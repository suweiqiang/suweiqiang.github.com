---
layout: post
title: Ubuntu 安装 Sublime Text
permalink: ubuntu-install-sublime-text
lastmod: 2013-07-21 00:00:00
category: 记录
tags: ubuntu install sublime-text
---

## 添加源方式安装

    sudo add-apt-repository ppa:webupd8team/sublime-text-2
    sudo apt-get update
    sudo apt-get install sublime-text

想要 Sublime Text 2 打开默认文本，可以修改关联列表

    sudo vim /usr/share/applications/defaults.list

将所有的 gedit.desktop 替换成 sublime-text-2.desktop

## 解决中文输入法问题

我用的是 fcitx-sogoupinyin 输入法，无法输入中文，按如下办法可行

1.  保存下述代码为 sublime-imfix.c 文件

    ```c
    /*
    sublime-imfix.c
    Use LD_PRELOAD to interpose some function to fix sublime input method support for linux.
    By Cjacker Huang <jianzhong.huang at i-soft.com.cn>

    gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
    LD_PRELOAD=./libsublime-imfix.so sublime_text
    */
    #include <gtk/gtk.h>
    #include <gdk/gdkx.h>
    typedef GdkSegment GdkRegionBox;

    struct _GdkRegion
    {
      long size;
      long numRects;
      GdkRegionBox *rects;
      GdkRegionBox extents;
    };

    GtkIMContext *local_context;

    void
    gdk_region_get_clipbox (const GdkRegion *region,
                GdkRectangle    *rectangle)
    {
      g_return_if_fail (region != NULL);
      g_return_if_fail (rectangle != NULL);

      rectangle->x = region->extents.x1;
      rectangle->y = region->extents.y1;
      rectangle->width = region->extents.x2 - region->extents.x1;
      rectangle->height = region->extents.y2 - region->extents.y1;
      GdkRectangle rect;
      rect.x = rectangle->x;
      rect.y = rectangle->y;
      rect.width = 0;
      rect.height = rectangle->height;
      //The caret width is 2;
      //Maybe sometimes we will make a mistake, but for most of the time, it should be the caret.
      if(rectangle->width == 2 && GTK_IS_IM_CONTEXT(local_context)) {
            gtk_im_context_set_cursor_location(local_context, rectangle);
      }
    }

    //this is needed, for example, if you input something in file dialog and return back the edit area
    //context will lost, so here we set it again.

    static GdkFilterReturn event_filter (GdkXEvent *xevent, GdkEvent *event, gpointer im_context)
    {
        XEvent *xev = (XEvent *)xevent;
        if(xev->type == KeyRelease && GTK_IS_IM_CONTEXT(im_context)) {
           GdkWindow * win = g_object_get_data(G_OBJECT(im_context),"window");
           if(GDK_IS_WINDOW(win))
             gtk_im_context_set_client_window(im_context, win);
        }
        return GDK_FILTER_CONTINUE;
    }

    void gtk_im_context_set_client_window (GtkIMContext *context,
              GdkWindow    *window)
    {
      GtkIMContextClass *klass;
      g_return_if_fail (GTK_IS_IM_CONTEXT (context));
      klass = GTK_IM_CONTEXT_GET_CLASS (context);
      if (klass->set_client_window)
        klass->set_client_window (context, window);

      if(!GDK_IS_WINDOW (window))
        return;
      g_object_set_data(G_OBJECT(context),"window",window);
      int width = gdk_window_get_width(window);
      int height = gdk_window_get_height(window);
      if(width != 0 && height !=0) {
        gtk_im_context_focus_in(context);
        local_context = context;
      }
      gdk_window_add_filter (window, event_filter, context);
    }
    ```

2.  安装 C/C++ 的编译环境和 gtk libgtk2.0-dev

        sudo apt-get install build-essential
        sudo apt-get install libgtk2.0-dev

3.  编译共享内库

        gcc -shared -o libsublime-imfix.so sublime-imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC

4.  启动 Sublime Text 2

        LD_PRELOAD=./libsublime-imfix.so sublime-text

但是这样的话，我们每次都要在终端里面使用命令启动 sublime text 2，这样很不方便，
接下来我们还要通过修改 sublime-text-2.desktop 达到点击图标启动

    sudo mv libsublime-imfix.so /usr/lib
    sudo vim /usr/share/applications/sublime-text-2.desktop

将

    Exec=/usr/bin/subl %F

修改为

    Exec=bash -c 'LD_PRELOAD=/usr/lib/libsublime-imfix.so /usr/bin/subl %F'

将

    Exec=/usr/bin/subl --command new_file

修改为

    Exec=bash -c 'LD_PRELOAD=/usr/lib/libsublime-imfix.so /usr/bin/subl' --command new_file

好了，接下来你在 dash 中点击打开 sublime-text-2 吧，开始你的代码之旅吧

- - -

参考：  
<http://my.oschina.net/wugaoxing/blog/121281>
