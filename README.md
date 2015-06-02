# FPSListView

If you want to calculate the frame rate of ListView, the following method you can use it to calculte the FPS of ListView
roughly.

<pre>
public class FPSListView extends ListView {
    private static final String DEBUG = "FPSListView";

    private Toast fpsToast;
    private long mFpsStartTime = -1;
    private long mFpsPrevTime = -1;
     private int mFpsNumFrames;

    public FPSListView(Context context) {
        super(context);
        fpsToast = Toast.makeText(context, "", Toast.LENGTH_SHORT);
    }

    public FPSListView(Context context, AttributeSet attrs) {
        super(context, attrs);
        fpsToast = Toast.makeText(context, "", Toast.LENGTH_SHORT);
    }

    private void trackFPS() {
        // Tracks frames per second drawn. First value in a series of draws may be bogus
        // because it down not account for the intervening idle time
        long nowTime = System.currentTimeMillis();
        if (mFpsStartTime < 0) {
            mFpsStartTime = mFpsPrevTime = nowTime;
            mFpsNumFrames = 0;
        } else {
            ++mFpsNumFrames;
            long totalTime = nowTime - mFpsStartTime;
            mFpsPrevTime = nowTime;

            if (totalTime > 1000) {
                float fps = (float) mFpsNumFrames * 1000 / totalTime;
                mFpsStartTime = nowTime;
                mFpsNumFrames = 0;
                Log.d(DEBUG, "FPS = " + fps);
                fpsToast.setText("FPS: " + fps);
                fpsToast.show();
            }
        }
    }

    @Override
    public void draw(Canvas canvas) {
        super.draw(canvas);
        trackFPS();
    }
}
</pre>

<br/> <H1>[Methodology]</H1>

<b> mFpsNumFrames (Frames) during deltaTime(ms) = nowTime - mFpsStartTime , so the FPS = mFpsNumFrames/(delta * 10 ^ -3) = mFpsNumFrames * 1000 / delta ##<b/> 
