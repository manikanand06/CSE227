package com.example.graphicsdemo;

import android.annotation.SuppressLint;
import android.content.Context;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.graphics.Path;
import android.util.AttributeSet;
import android.util.Log;
import android.view.MotionEvent;
import android.view.SurfaceHolder;
import android.view.SurfaceHolder.Callback;
import android.view.SurfaceView;
import android.widget.Toast;

import java.sql.ResultSet;


/**
 * Created by USER on 05-Sep-18.
 */

public class MySurfaceView extends SurfaceView implements Runnable {

    boolean isrunning;
    Thread mthread;

    Context ct;
    SurfaceHolder sh;
    Paint mpaint;


    Bitmap bitmap;
    int mx = 0, my;

    Canvas canvas;

    public MySurfaceView(Context context) {
        super(context);
        init(context);
    }

    public MySurfaceView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context);
    }

    @SuppressLint("WrongCall")
    @Override
    public void run() {

        Log.d("Tag", "inside run");
        while (isrunning) {
            try {
                Thread.sleep(200);
                canvas = sh.lockCanvas();

                onDraw(canvas);

            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                if (canvas != null) {
                    sh.unlockCanvasAndPost(canvas);
                    postInvalidate();

                }
            }

        }
    }

    void init(Context ct) {

        this.ct = ct;
        mpaint = new Paint();

        bitmap = BitmapFactory.decodeResource(getResources(), R.drawable.image1);
        Toast.makeText(ct, "Inside init", Toast.LENGTH_SHORT).show();

        sh = getHolder();

        sh.addCallback(new Callback() {
            @SuppressLint("WrongCall")
            @Override
            public void surfaceCreated(SurfaceHolder surfaceHolder) {

                //canvas = sh.lockCanvas();
                //onDraw(canvas);
                //sh.unlockCanvasAndPost(canvas);

                my = getHeight()/2;
            //    resume();
            }

            @Override
            public void surfaceChanged(SurfaceHolder surfaceHolder, int i, int i1, int i2) {

            }

            @Override
            public void surfaceDestroyed(SurfaceHolder surfaceHolder) {
                stop();
            }
        });

    }

    void resume() {
        isrunning = true;
        mthread = new Thread(this);
        mthread.start();
    }

    void stop() {
        isrunning = false;
        try {
            mthread.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    @Override
    protected void onDraw(Canvas canvas)
    {
        canvas.drawBitmap(bitmap, mx, my, mpaint);
       /* mx = mx + 10;
        if (mx >= getWidth()) {
            mx = 0;
        }*/

    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        if(event.getAction() == MotionEvent.ACTION_DOWN)
        {
            mx  = (int)event.getX();
            my = (int) event.getY();
            resume();
        }

        return true;
    }
}