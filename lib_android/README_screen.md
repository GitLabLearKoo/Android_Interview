# android 屏幕相关 总结
 
## 为什么要屏幕适配
 为了保证用户获得一致的用户体验效果,使得某一元素在Android不同尺寸、不同分辨率的、不同系统的 手机上具备相同的显示效果，
 能够保持界面上的效果一致,我们需要对各种手机屏幕进行适配!
 
 Android**系统碎片化**:基于Google原生系统，小米定制的MIUI、魅族定制的flyme、华为定制的 EMUI等等;
 
 Android机型**屏幕尺寸碎片化**:5寸、5.5寸、6寸等等; 
 
 Android屏幕**分辨率碎片化**:320x480、480x800、720x1280、1080x1920等。
 
## 基本概念

1. 像素(px):像素就是手机屏幕的最小构成单元，px = 1像素点 一般情况下UI设计师的设计图会 以px作为统一的计量单位。
2. 分辨率:手机在横向、纵向上的像素点数总和 一般描述成 宽*高 ，即横向像素点个数 * 纵向像素 点个数(如1080 x 1920)，单位:px。
3. 屏幕尺寸:手机对角线的物理尺寸。单位 英寸(inch)，一英寸大约2.54cm 常见的尺寸有4.7 寸、5寸、5.5寸、6寸。
4. 屏幕像素密度(dpi):每英寸的像素点数，例如每英寸内有160个像素点，则其像素密度为 160dpi，单位:dpi(dots per inch)。
5. 标准屏幕像素密度(mdpi): 每英寸长度上还有160个像素点(160dpi)，即称为标准屏幕像素 密度(mdpi)。 
6. **密度无关像素(dp)**:与终端上的实际物理像素点无关，可以保证在不同屏幕像素密度的设备上 显示相同的效果，是安卓特有的长度单位，dp与px的转换:**1dp = (dpi / 160 ) * 1px**¬。 
7. **独立比例像素(sp)**:字体大小专用单位 Android开发时用此单位设置文字大小，推荐使用 12sp、14sp、18sp、22sp作为字体大小。

### （面试）dp是什么，sp呢，有什么区别

## 适配方案
适配的最多的2个分辨率:1280*800,1920*1080 

解决方案: 对于Android的屏幕适配，我认为可以从以下4个方面来做: 
1. 布局的组件适配

请务必使用密度无关像素 dp 或独立比例像素 sp 单位指定尺寸。 

使用相对布局或线性布局，不要使用绝对布局
            
使用wrap_content、match_parent、权重
 
 使用minWidth、minHeight、lines等属性
 
dimens使用:

不同的屏幕尺寸可以定义不同的数值，或者是不同的语言显示我们也可以定义不同的数值，因为翻译后 的长度一般都不会跟中文的一致。使用新的约束布局。

2. 布局适配 

使用限定符(屏幕密度限定符、尺寸限定符、最小宽度限定符、布局别名、屏幕方向限定符)根据屏幕的配置来加载相应的UI布局。

3. 图片资源适配

使用自动拉伸图.9png图片格式使图片资源自适应屏幕尺寸。

普通图片和图标:

建议按照官方的密度类型进行切图即可，但一般我们只需xxhdpi或xxxhdpi的切图即可满足我们的需 求;

4. 代码适配: 在代码中使用Google提供的API对设备的屏幕宽度进行测量，然后按照需求进行设置。 
5. 接口配合: 本地加载图片前判断手机分辨率或像素密度，向服务器请求对应级别图片。

https://github.com/JessYanCoding/AndroidAutoSize

https://github.com/JessYanCoding/MVPArms

使用介绍：

https://juejin.im/post/5bce688e6fb9a05cf715d1c2


## 多语言文件的配置

res/value res/value-zh res/value-en

    
    //代码内设置默认语言
    private void setLanguage() {
        ACache aCache = ACache.get(this);//使用ACache保存配置的语言
        //如果系统当前默认的语言不等于当前ACache中保存的语言，就更改
        if (Locale.getDefault() != Utils.getLanguageLocal(aCache.getAsString(Content.currentLanguage))){
            DisplayMetrics displayMetrics = getResources().getDisplayMetrics();
            Configuration configuration = getResources().getConfiguration();
            Locale locale = Utils.getLanguageLocal(aCache.getAsString(Content.currentLanguage));//获取到ACache中保存的语言
            configuration.setLocale(locale);//设置语言
            getResources().updateConfiguration(configuration, displayMetrics);//更新系统配置文件
        }
    }
