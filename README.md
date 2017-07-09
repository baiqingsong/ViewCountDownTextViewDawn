# 自定义倒计时文本使用

* [代码使用](#代码使用)


## 代码使用
xml中代码：
```
<FrameLayout
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_alignParentRight="true"
    android:layout_alignParentTop="true">

    <TextView
        android:id="@+id/start_skip_count_down"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="10dp"
        android:text="跳过"
        android:gravity="center"
        android:background="@drawable/bg_start_page_circle"
        android:textColor="@android:color/white"
        android:textSize="14sp"
        />
</FrameLayout>
```
这段是xml中的主要代码，一般设置在进入页面（相对布局）中。

其中TextView的背景设置：
```
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">

    <solid android:color="#80000000"/>

    <padding
        android:bottom="3dp"
        android:left="8dp"
        android:right="8dp"
        android:top="3dp"/>

    <corners
        android:bottomLeftRadius="45dp"
        android:bottomRightRadius="45dp"
        android:topLeftRadius="45dp"
        android:topRightRadius="45dp"/>

</shape>
```
这段代码主要是设置圆角颜色和边距

Java代码：
```
public class ScreenOpenActivity extends AppCompatActivity {
    private TextView mCountDownTextView;
    private Handler mHandler = new Handler();
    private CustomCountDownTimer mCountDownTimer;
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_screen_open);
        mCountDownTextView = (TextView) findViewById(R.id.start_skip_count_down);
        mCountDownTextView.setText("3s 跳过");
        //创建倒计时
        //创建倒计时类
        mCountDownTimer = new CustomCountDownTimer(3000, 1000);
        mCountDownTimer.start();
        //3秒后跳转或者点击跳转到主界面
        mHandler.postDelayed(runnable, 3000);
        mCountDownTextView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                jumpToMain();
                mHandler.removeCallbacks(runnable);
            }
        });
    }
    Runnable runnable = new Runnable() {
        @Override
        public void run() {
            jumpToMain();
        }
    };

    /**
     * 跳转到主界面
     */
    private void jumpToMain(){
        startActivity(new Intent(this, MainActivity.class));
        finish();
    }
    class CustomCountDownTimer extends CountDownTimer {
        /**
         * @param millisInFuture
         *      表示以「 毫秒 」为单位倒计时的总数
         *      例如 millisInFuture = 1000 表示1秒
         *
         * @param countDownInterval
         *      表示 间隔 多少微秒 调用一次 onTick()
         *      例如: countDownInterval = 1000 ; 表示每 1000 毫秒调用一次 onTick()
         *
         */

        public CustomCountDownTimer(long millisInFuture, long countDownInterval) {
            super(millisInFuture, countDownInterval);
        }


        public void onFinish() {
            mCountDownTextView.setText("0s 跳过");
        }

        public void onTick(long millisUntilFinished) {
            mCountDownTextView.setText( millisUntilFinished / 1000 + "s 跳过");
        }

    }
    @Override
    protected void onDestroy() {
        if (mCountDownTimer != null) {
            mCountDownTimer.cancel();
        }
        super.onDestroy();
    }
}
```
这段代码主要就是一个倒计时实现。
