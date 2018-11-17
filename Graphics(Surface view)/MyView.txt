package com.example.graphicsdemo;

import android.content.Context;
import android.graphics.drawable.Drawable;
import android.support.annotation.Nullable;
import android.support.v4.content.res.ResourcesCompat;
import android.support.v7.widget.AppCompatEditText;
import android.text.Editable;
import android.text.TextWatcher;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.view.View;
import android.widget.Toast;

/**
 * Created by USER on 03-Sep-18.
 */


public class MyView extends AppCompatEditText
{
    Drawable clearButton, clearButton1;

    public MyView(Context context) {
        super(context);
        init();
    }

    public MyView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        init();
    }


    private void init()
    {
        clearButton = ResourcesCompat.getDrawable(getResources(),R.drawable.ic_clear_black_24dp,null);

        clearButton1 = ResourcesCompat.getDrawable(getResources(), R.drawable.ic_clear_opackblack_24dp, null);

        addTextChangedListener(new TextWatcher() {
            @Override
            public void beforeTextChanged(CharSequence charSequence, int i, int i1, int i2) {

            }

            @Override
            public void onTextChanged(CharSequence charSequence, int i, int i1, int i2) {

                showButton();

            }

            @Override
            public void afterTextChanged(Editable editable) {

            }
        });


        setOnTouchListener(new OnTouchListener() {
            @Override
            public boolean onTouch(View view, MotionEvent motionEvent) {

                float clearButtonstart;

                boolean isclicked = false;

                clearButtonstart = getWidth()-getPaddingEnd()- clearButton.getIntrinsicWidth();

                if(motionEvent.getX() > clearButtonstart)
                {
                    isclicked = true;
                }
                if(isclicked) {
                    switch (motionEvent.getAction()) {
                        case MotionEvent.ACTION_DOWN:
                            getText().clear();
                            break;
                        case MotionEvent.ACTION_UP:
                            hideButton();
                            break;
                    }
                }
                return true;
            }
        });
    }

    void showButton()
    {
        setCompoundDrawablesRelativeWithIntrinsicBounds(null,null,clearButton,null);

    }

    void hideButton()
    {
        setCompoundDrawablesRelativeWithIntrinsicBounds(null,null,clearButton1,null);
    }

};