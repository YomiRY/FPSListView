# FPSListView

If you want to calculate the frame rate of ListView, the following method you can use it to calculte the FPS of ListView
roughly.

<pre>
public class FPSListView extends ListView
{
    private Toast fpsToast;
    private long currentTime;
    private long prevTime;

    public FPSListView(Context context){
        super(context);
        fpsToast = Toast.makeText(context, "", Toast.LENGTH_SHORT);
    }

    public FPSListView(Context context, AttributeSet attrs){
        super(context, attrs);
        fpsToast = Toast.makeText(context, "", Toast.LENGTH_SHORT);
    }

    @Override
    protected void onDraw(Canvas canvas) {          
        super.onDraw(canvas);

        currentTime = SystemClock.currentThreadTimeMillis();
        long deltaTime = currentTime - prevTime;
        long fps = 1000/deltaTime;
        fpsToast.setText("FPS: " + fps);
        fpsToast.show();
        prevTime = currentTime;
    }
}
</pre>

<br/> <H1>[Methodology]</H1>

<H3> 1 Frame = deltaTime(ms), so the FPS = 1/(delta * 10 ^ -3) = 1000 / delta ## </H3>
