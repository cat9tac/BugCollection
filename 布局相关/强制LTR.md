# 强制LTR
在RTL的情况下也有要求强制LTR的，那就直接在view上进行修改吧
```sh
private static final int LAYOUT_DIRECTION_LTR = 0;
private static final int TEXT_DIRECTION_LTR = 3;
//获取系统语言
currentLocale= mActivity.getResources().getConfiguration().locale.getLanguage();
        if("ur"==currentLocale){
            //设置布局方向
            ret.setLayoutDirection(LAYOUT_DIRECTION_LTR);
        }
         if("ur"==currentLocale){
            //设置文字方向
            labelView.setTextDirection(TEXT_DIRECTION_LTR);
        }
```