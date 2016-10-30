---
title: File类和IO流相关常用api的练习
date: 2016-10-27 18:22:07
tags: [IO,File]
categories: JavaSE
---
## 利用File类和IO相关的api实现了一些基本的系统功能
** show my codes **：
<!--more-->
```java
package io.ondelete.file;

import java.io.*;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * Created by southwe
 * 以下是对File类和IO流的基本api的一些小练习
 */
public class FileUtils {

    public static void newFile(String path) {
        File file = new File(path);
        if (!file.exists()) {
            try {
                System.out.println("file created! " + file.createNewFile());
                System.out.println("file can read. " + file.canRead());
                System.out.println("file can write. " + file.canWrite());
                System.out.println("file can excute. " + file.canExecute());
                System.out.println("file length:" + file.length());
                System.out.println(file);
            } catch (IOException e) {
                e.printStackTrace();
            }
        } else {
            System.out.println("file deleted! " + file.delete());
            file.deleteOnExit();
        }
        System.out.println("separator-->" + File.separator);//文件分割符：windows下是"\",unix下是"/"
        System.out.println("separatorChar-->" + File.separatorChar);
        System.out.println("pathSeparator-->" + File.pathSeparator);//路径分割符：windows下是";",unix下是":"(在环境变量PTAH配置时可见)
        System.out.println("pathSeparatorChar-->" + File.pathSeparatorChar);
    }


    /**
     * 列出指定的文件或文件夹路径 unix风格
     *
     * @param path 指定要列出的文件或文件夹路径
     */
    public static void ls(String path) {
        File file = new File(path);
        if (!file.exists()) {
            System.out.println("dir make! " + file.mkdirs());
        } else {
            File[] files = file.listFiles();
            for (File f : files
                    ) {
                String isdir = f.isDirectory() ? "d" : "-";
                String canread = f.canRead() ? "r" : "-";
                String canwrite = f.canWrite() ? "w" : "-";
                String canexcute = f.canExecute() ? "x" : "-";
                long time = f.lastModified();
                Date date = new Date(time);
                DateFormat format = new SimpleDateFormat("MM月 dd HH:mm");
                String formattime = format.format(date);
                System.out.println(isdir + canread + canwrite + canexcute + " "
                        + f.length() + " " + formattime + " " + f.getName());
            }
        }
    }

    /**
     * 实现文件的拷贝
     *
     * @param src  原文件路径
     * @param dest 目标文件路径
     * @throws IOException
     */
    public static void cpFile(File src, File dest) throws IOException {
        if (!dest.exists()) {
            try {
                dest.createNewFile();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        InputStream is = null;
        OutputStream os = null;
        try {
            is = new FileInputStream(src);
            os = new FileOutputStream(dest, true);//后面的true表示追加操作，默认为false，在写入时覆盖原有的数据。
            byte[] flush = new byte[1024];
            int len = 0;
            while (-1 != (len = is.read(flush))) {
                //
                //is.read(flush,0,len)
                os.write(flush, 0, len);
            }
            os.flush();
        } catch (IOException e) {
            throw e;
        } catch (FileNotFoundException e) {
            throw e;
        } finally {
            if (null != is && null != os) {
                try {
                    os.close();
                    is.close();
                } catch (IOException e) {
                    throw e;
                }
            }
        }
    }

    /**
     * 实现对一个文件夹的拷贝
     *
     * @param src  原文件或者文件夹对象
     * @param dest 目标文件或者文件夹对象
     */
    public static void cpdir(File src, File dest) {
        //判断src对象是否是一个文件，如果为一个文件，则直接创建
        if (src.isFile()) {
            try {
                cpFile(src, dest);
            } catch (IOException e) {
                e.printStackTrace();
            } catch (FileNotFoundException e) {
            throw e;
            }
        } else if (src.isDirectory()) {
            //如果为一个目录，首先创建dest文件夹
            if (!dest.exists()) {
                dest.mkdir();
            }
            //然后遍历src文件夹，查看其子内容时文件夹还是文件，递归调用本方法，达到拷贝文件夹的目的
            for (File sub :
                    src.listFiles()) {
                cpdir(sub, new File(dest, sub.getName()));
            }
        }
    }

    /**
     * @param srcpath  原文件或者文件夹路径
     * @param destpath 目标文件或者文件夹路径
     */
    public static void cpdir(String srcpath, String destpath) {
        cpdir(new File(srcpath), new File(destpath));
    }
```