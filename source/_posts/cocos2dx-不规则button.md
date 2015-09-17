title: cocos2dx 不规则button
date: 2015-09-17 14:45:43
tags:
---

##UIWidget.h
```cpp
public:
	virtual bool AlphaTouchCheck(const Vec2 &point);
	virtual bool getAlphaTouchEnable();
	virtual void setAlphaTouchEnable(bool isAlphaTouch);
private:
	bool _isAlphaTouchEnable = false;
```
##UIWidget.cpp
```cpp
bool Widget::AlphaTouchCheck(const Vec2 &point)
{
	return true;
}

bool Widget::getAlphaTouchEnable()
{
	return _isAlphaTouchEnable;
}

void Widget::setAlphaTouchEnable(bool isAlphaTouch)
{
	_isAlphaTouchEnable = isAlphaTouch;
}
```
##UIButton.h
```cpp
public:
	bool AlphaTouchCheck(const Vec2 &point);
```
##UIButton.cpp
```cpp
bool Button::AlphaTouchCheck(const Vec2& point)
{
	if (getAlphaTouchEnable() && FileUtils::getInstance()->isFileExist("res/resources/mapicon/" + _normalFileName))
	{
		Image* normalImage = new Image();
		normalImage->initWithImageFile("res/resources/mapicon/" + _normalFileName);
		auto data = normalImage->getData();
		if (data == NULL)//|| std::strlen((char *)data) < 10)
		{
			CC_SAFE_DELETE(normalImage);
			return true;
		}
		auto locationInNode = this->convertToNodeSpace(point);
		//图片的0，0 是左上角 所以要和触摸点的Y转换一下 也就是“(normalImage->getHeight() - (int)(locationInNode.y) - 1)” 
		//该data值是把二维数组展开成一个一维数组, 因为每个像素值由RGBA组成, 所以每隔4个char为一个RGBA, 并且像素以横向排列
		int pa = 4 * ((normalImage->getHeight() - (int)(locationInNode.y) - 1) * normalImage->getWidth() + (int)(locationInNode.x)) + 3;
		unsigned int ap = data[pa];
		if (ap < 20)
		{
			CC_SAFE_DELETE(normalImage);
			return false;
		}
		else
		{
			CC_SAFE_DELETE(normalImage);
			return true;
		}
	}
	return true;
}
```
-----------------
站在某巨人的肩膀上!