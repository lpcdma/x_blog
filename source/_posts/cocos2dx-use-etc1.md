title: "cocos2dx use etc1"
date: 2015-08-30 01:37:33
tags:
---
#图片处理
```
etcpack C:\Users\peng\Desktop\fffffff.png C:\Users\peng\Desktop -s fast -as -c etc1 -ext PNG
```
#着色器
##etc_shader.frag
```
varying vec4 v_fragmentColor;
varying vec2 v_texCoord;
  
uniform sampler2D u_texture1;

void main()
{
	vec4 color = texture2D(CC_Texture0, v_texCoord);
	color.a = texture2D(u_texture1, v_texCoord).r;

    gl_FragColor = color;
}
```
##etc_shader.vert
```
attribute vec4 a_position;
attribute vec4 a_color;
attribute vec2 a_texCoord;
  
varying vec4 v_fragmentColor;
varying vec2 v_texCoord;
  
void main()
{
    gl_Position = CC_PMatrix * a_position;
    v_fragmentColor = a_color;
    v_texCoord = a_texCoord;
}
```
##cpp code
```
auto textureAlpha = Director::getInstance()->getTextureCache()->addImage("fffffff_alpha.pkm");

auto program = GLProgram::createWithFilenames("etc_shader.vert", "etc_shader.frag");
auto pstate = GLProgramState::create(program);
pstate->setUniformTexture("u_texture1", textureAlpha);

SpriteFrameCache::getInstance()->addSpriteFramesWithFile("ffff.plist");

CCLOG("lpcdma2 %s", Director::getInstance()->getTextureCache()->getCachedTextureInfo().c_str());

auto sprite3 = Sprite::createWithSpriteFrameName("f.png");
sprite3->setGLProgramState(pstate);
sprite3->setPosition(Vec2(600, 100));
this->addChild(sprite3);
```

待解决的问题:
每个使用这种方式每个node都要去设置一次setGLProgramState,这样很麻烦,可控性差.
参考:
<我所理解的cocos2d-x>