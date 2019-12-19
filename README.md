# SlidingPaneLayout-Left-Right-
SlidingPaneLayout_Left&amp;right_安卓左滑&amp;右滑关闭
![效果](https://github.com/DecentChunibyoPatient/SlidingPaneLayout-Left-Right-/blob/master/1.gif)

#### 主要代码
@Override
    public void onClick(View v) {
        // 是否需要重启
        boolean restart;
        switch (v.getId()) {
            // 跟随系统
            case R.id.btn_language_auto:
                restart = LanguagesManager.setSystemLanguage(this);
                break;
            // 简体中文
            case R.id.btn_language_cn:
                restart = LanguagesManager.setAppLanguage(this, Locale.CHINA);
                break;
            // 繁体中文
            case R.id.btn_language_tw:
                restart = LanguagesManager.setAppLanguage(this, Locale.TAIWAN);
                break;
            // 英语
            case R.id.btn_language_en:
                restart = LanguagesManager.setAppLanguage(this, Locale.ENGLISH);
                break;
            default:
                restart = false;
                break;
        }

        if (restart) {
            // 我们可以充分运用 Activity 跳转动画，在跳转的时候设置一个渐变的效果
            startActivity(new Intent(this, LanguageActivity.class));
            overridePendingTransition(R.anim.activity_alpha_in, R.anim.activity_alpha_out);
            finish();
        }
    }
