# LeranValueAnimator
Android属性动画相关API  ValueAnimator实例

```java
public class MainActivity extends AppCompatActivity {

    private LinearLayout layout;
    private ImageView img;
    private int width;
    private int height;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        layout = (LinearLayout) findViewById(R.id.layout);
        img = (ImageView) findViewById(R.id.img);
    }

    public void onBtn1(View v) {
        lineAnimator();
    }

    public void onBtn2(View v) {
        scaleAnimator();
    }

    public void onBtn3(View v) {
        raAnimator();
    }
    // 一个修改ImageView位置的方法
    private void moveImg(View view, int rawX, int rawY) {
        int left = rawX - img.getWidth() / 2;
        int top = rawY - img.getHeight();
        int w = left + view.getWidth();
        int h = view.getHeight();
        view.layout(left, top, w, h);
    }
    // 按轨迹方程来运动
    private void lineAnimator() {
        width = layout.getWidth();
        height = layout.getHeight();
        ValueAnimator animator = ValueAnimator.ofInt(height, 0, height / 4, height / 2, height / 4 * 3, height);
        animator.setDuration(6000);
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                // 轨迹方程
                int y = (int) valueAnimator.getAnimatedValue();
                int x = width / 2;
                moveImg(img, x, y);
            }
        });
        animator.setInterpolator(new LinearInterpolator());
        animator.start();
    }
    //缩放效果
    private void scaleAnimator() {
        final float scale = 0.5f;
        final ValueAnimator animator = ValueAnimator.ofFloat(1.0f, scale);
        animator.setDuration(1500);
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                float scale = valueAnimator.getAnimatedFraction();
                img.setScaleX(scale);
                img.setScaleY(scale);
            }
        });
        animator.setInterpolator(new LinearInterpolator());
        animator.start();
    }
    //旋转同时透明度发生变化
    private void raAnimator() {
        ValueAnimator animator = ValueAnimator.ofInt(0, 360);
        animator.setDuration(1000L);
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                int rotate = (int) valueAnimator.getAnimatedValue();
                img.setRotation(rotate);
                float fraction = valueAnimator.getAnimatedFraction();
                img.setAlpha(fraction);
            }
        });
        animator.setInterpolator(new LinearInterpolator());
        animator.start();
    }
}
```
