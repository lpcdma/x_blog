title: "listview在android上会出现背景、样式等消失。"
date: 2014-10-10 16:28:58
tags:
id: 739
comment: false
categories:
  - 编程学习
---

问题描述，

https://github.com/cocos2d/cocos2d-x/issues/8618#issuecomment-58623524

cpp代码：
<pre class="brush:cpp">Node* node = static_cast(cocostudio::GUIReader::getInstance()-&gt;widgetFromJsonFile("fuckbg/fuckbg.ExportJson"));
this-&gt;addChild(node, 0);</pre>
解决：

**for Android:**in game activity:
<pre class="brush:java">public Cocos2dxGLSurfaceView onCreateView() {
        Cocos2dxGLSurfaceView glSurfaceView = new Cocos2dxGLSurfaceView(this);
        glSurfaceView.setEGLConfigChooser(5, 6, 5, 0, 16, 8);
        return glSurfaceView;
    }</pre>
GLSurfaceView说明：

http://developer.android.com/reference/android/opengl/GLSurfaceView.html#setEGLConfigChooser(int, int, int, int, int, int)

**for iOS:**in AppController replace the gl-view creation with:
<pre class="brush:cpp">EAGLView *__glView = [EAGLView viewWithFrame: [window bounds]
                                     pixelFormat: kEAGLColorFormatRGBA8
                                     depthFormat: GL_DEPTH24_STENCIL8_OES
                              preserveBackbuffer: NO
                                      sharegroup: nil
                                   multiSampling: NO
                                 numberOfSamples: 0];</pre>