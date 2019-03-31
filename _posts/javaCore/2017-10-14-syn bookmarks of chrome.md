---
layout: default
title: 一种同步chrome 书签的方法
---

国内因为某些原因，chrome 浏览器的同步总是不能使用，尝试了其他书签同步的工具，如 `xmarks`，但是这个东西经常会同步异常，合并得乱七八糟。  
今天周末，思考这个问题，困扰许久，就是需要公司与家的chrome 能有共同的设置，操作能有流畅感。  
所以决定将chrome 书签同步至服务器，公司与家，都能享受流畅的操作。那就拿出老本行，写段代码搞喽  
首先想到用python, 这货简单易用，查了有个库`pyinotify`可以满足要求，安装此库才发现居然只有64位的，可怜了我七年高龄的win7，无耐，只好用上班那套了，java 走起。

同步书签需要有以下必需几步骤：

1. 检测到书签变化，chrome 中即感知到文件变化即可；
2. 将书签发送至远程服务器；
3. 将远程书签下载到本地；

目前（2017-10-14 18:19:24）已经实现了前两步，第三步还没想好怎么处理。下面详细说下实现过程：

检测到书签变化
=
用来apache 的 `FileAlterationListenerAdaptor` 实现其中的监控文件变化方法，具体如下

```java
public class FileMonitor extends FileAlterationListenerAdaptor
{

	private static FileMonitor fileMonitor;

	private FileMonitor()
	{

	}

	// Get singleton object instance
	public static FileMonitor getFileMonitor()
	{
		if (fileMonitor == null)
		{
			synchronized (FileMonitor.class)
			{
				if (fileMonitor == null)
				{
					fileMonitor = new FileMonitor();
				}
			}
		}

		return fileMonitor;
	}

	// Change file event
	@Override
	public void onFileChange(File file)
	{
		String inspectFile = LeonConfig.getProperties("inspectFile");
		if ((file.getAbsolutePath() + ",").contains(inspectFile + ","))
		{
			// 只对这个文件做监控
			String desPath = LeonConfig.getProperties("desDirectory") + inspectFile;
			File desFile = new File(desPath);
			try
			{
				FileOutputStream out = new FileOutputStream(desFile);
				FileInputStream in = new FileInputStream(file);
				byte[] buff = new byte[1024];
				int c = 0;
				while ((c = in.read(buff, 0, 1024)) > 0)
				{
					out.write(buff, 0, c);
				}
				out.close();
				in.close();
				GitUtilClass.commitFiles();
			}
			catch (Exception e)
			{
				e.printStackTrace();
			}
		}
		if ((file.getAbsolutePath() + ",").contains("test,"))
			System.out.println("[Change]: " + file.getAbsolutePath());
	}

	// Delete file event
	@Override
	public void onFileDelete(File file)
	{
		//		System.out.println("[Delete]: " + file.getAbsolutePath());
	}

	public void monitor(String directory, int interval)
	{
		//		System.out.println('t');
		// Observer file whose suffix is pm 
		FileAlterationObserver fileAlterationObserver = new FileAlterationObserver(directory, FileFilterUtils.and(FileFilterUtils.fileFileFilter(), FileFilterUtils.suffixFileFilter("")), null);

		// Add listener for event (file create & change & delete)
		fileAlterationObserver.addListener(this);

		// Monitor per interval
		FileAlterationMonitor fileAlterationMonitor = new FileAlterationMonitor(interval, fileAlterationObserver);

		try
		{
			// Start to monitor
			fileAlterationMonitor.start();
		}
		catch (Exception e)
		{
			e.printStackTrace();
		}
	}
}
```


将书签同步至远程服务器
=
这个需要用java 做网络请求，又需要找个服务器存数据，思来想去，用git 做挺好，自带版本控制，而且代码就是用git 管理，想来保存书签也不会太麻烦了，于是google，有个神奇的jar --`jgit`， 牛逼，赶紧下载测试，发现完全可以，下面是代码：

```

import java.io.File;
import java.io.IOException;
import java.util.Date;

import org.eclipse.jgit.api.Git;
import org.eclipse.jgit.api.errors.GitAPIException;

import com.leon.constant.LeonConfig;

public class GitUtilClass
{
	public static String localRepoGitConfig = LeonConfig.getProperties("gitRepDirectory") + ".git";

	public static void commitFiles() throws IOException, GitAPIException
	{
		String inspectFile = LeonConfig.getProperties("inspectFile");
		String filePath = LeonConfig.getProperties("gitRepDirectory") + inspectFile;
		Git git = Git.open(new File(localRepoGitConfig));
		//创建用户文件的过程
		File myfile = new File(filePath);
		myfile.createNewFile();
		git.add().addFilepattern(inspectFile).call();
		//提交
		git.commit().setMessage("backup " + new Date()).call();
		//推送到远程
		git.push().call();
	}

}
```
此处只做了Commit 操作，因为在同步过程中，暂时只用到commit &&push ，后面做下载时， 再pull。

总结
=
至此，监控文件变化与push操作都已经完了，很简单的代码。因为代码和书签保存在同一个内网git 上了，&&暂时还不想公式书签内容，完整代码可以关注我的公众号，回复 <font color="red">书签同步</font> 

