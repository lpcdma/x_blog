title: "使用UU云进行验证码识别(python)"
date: 2014-02-14 17:52:26
tags:
id: 499
comment: false
categories:
  - 学习笔记
---

自己做验证码识别确实江郎才尽，不过花点小钱可以事半功倍。
<pre class="brush:py">#encoding=UTF-8

import sys;
import os;
from ctypes import *;
import hashlib;

def GetFileMd5(strFile):  
    file = None;  
    bRet = False;  
    strMd5 = "";  

    try:  
        file = open(strFile, "rb");  
        md5 = hashlib.md5();  
        strRead = "";  

        while True:  
            strRead = file.read(8096);  
            if not strRead:  
                break;  
            md5.update(strRead);  
        #read file finish  
        bRet = True;  
        strMd5 = md5.hexdigest();  
    except:  
        bRet = False;  
    finally:  
        if file:  
            file.close()  
    return strMd5; 

#优优云DLL 文件MD5值校验
#用处：近期有不法份子采用替换优优云官方dll文件的方式，极大的破坏了开发者的利益
#用户使用替换过的DLL打码，导致开发者分成变成别人的，利益受损，
#所以建议所有开发者在软件里面增加校验官方MD5值的函数
#如何获取文件的MD5值，通过下面的GetFileMD5(文件)函数即返回文件MD5

md5=GetFileMd5('UUWiseHelper.dll');
#if(len(md5)&gt;0): #官方每次更新DLL，MD5值都会变化，您可以使用GetFileMd5函数获得文件的MD5值
#    print("对不起，您替换了图片识别文件。请下载官方原版。");
#    sys.exit(0);    #终止程序

# 全部函数列表：http://dll.uuwise.com/index.php?n=ApiDoc.AllFunc
# 技术QQ:87280085
# 加载动态链接库, 需要放在System 的path里，或者当前目录下
UU = windll.LoadLibrary('UUWiseHelper')

# 初始化函数调用
setSoftInfo = UU.uu_setSoftInfoW
login = UU.uu_loginW
recognizeByCodeTypeAndPath = UU.uu_recognizeByCodeTypeAndPathW
getResult = UU.uu_getResultW
uploadFile = UU.uu_UploadFileW
getScore = UU.uu_getScoreW
# 初始化函数调用

user_i = raw_input("Pleas input you user name and press enter:")
passwd_i = raw_input('Pleas input you user Password and press enter:')

s_id = c_int(20116)   # 授权 ID
s_key = c_wchar_p('d6cbaeccdaf84bd2803ab9e99cd2690b')  # 授权Key
user = c_wchar_p(user_i)  # 授权用户名
passwd = c_wchar_p(passwd_i)  # 授权密码

pic_file_path = os.path.join(os.path.dirname(__file__), 'test_pics', '6116113.png')
#此处指的当前路径下的test_pics文件夹下面的test.jpg
 #可以修改成你想要的文件路径

setSoftInfo(s_id, s_key)		#设置软件ID和KEY，仅需要调用一次即可，不需要每次上传图片都调用一次，特殊情况除外，比如当成脚本执行的话
ret = login(user, passwd)		#用户登录，仅需要调用一次即可，不需要每次上传图片都调用一次，特殊情况除外，比如当成脚本执行的话

if ret &gt; 0:
    print('login ok, user_id:%d' % ret)  #登录成功返回用户ID
else:
    print('login error')
    sys.exit(0)

ret = getScore(user, passwd)  #获取用户当前剩余积分
print('The Score of User : %s  is :%d' % (user.value, ret))

result=c_wchar_p("")
#code_id = recognizeByCodeTypeAndPath(c_wchar_p(pic_file_path),c_int(1006),result)
code_id = UU.uu_recognizeByCodeTypeAndUrlW(c_wchar_p('http://passport.wanmei.com/servlet/randImg4register'),c_int(0),c_int(1006),c_int(0),result)
if code_id &lt;= 0:
    print('get result error ,ErrorCode: %d' % code_id)
else:
    print("the resultID is :%d result is %s" % (code_id,result))  #识别结果为宽字符类型 c_wchar_p,运用的时候注意转换一下

raw_input('press any  Enter key to exit')</pre>