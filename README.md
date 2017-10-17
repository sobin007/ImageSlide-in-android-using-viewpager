# ImageSlide-in-android-using-viewpager

#Create project 

#Import 

  compile 'me.relex:circleindicator:1.2.2@aar'
  
  This is imported to indicate the viewpagers in bottom indicator
  
  #Add indicator codes here in main activity
  
    CircleIndicator indicator = (CircleIndicator)
                findViewById(R.id.indicator);

        indicator.setViewPager(mPager);


#MainActivity 

package touch.in.keep.sobin.viewpagersample;

import android.os.Handler;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

import java.util.ArrayList;
import java.util.Timer;
import java.util.TimerTask;

import me.relex.circleindicator.CircleIndicator;

public class MainActivity extends AppCompatActivity {
    
    private static ViewPager mPager;
    private static int currentPage = 0;
    private static int NUM_PAGES = 0;
    private static final Integer[] IMAGES= {R.drawable.p1,R.drawable.p2,R.drawable.p3,R.drawable.p4};
    private ArrayList<Integer> ImagesArray = new ArrayList<Integer>();
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        init();
    }
    
    private void init() {
        for(int i=0;i<IMAGES.length;i++)
            
            ImagesArray.add(IMAGES[i]);
        mPager = (ViewPager) findViewById(R.id.pager);
        mPager.setAdapter(new SlidingImageAdapter(MainActivity.this,ImagesArray));


        CircleIndicator indicator = (CircleIndicator)
                findViewById(R.id.indicator);

        indicator.setViewPager(mPager);

        final float density = getResources().getDisplayMetrics().density;

        NUM_PAGES =IMAGES.length;

      #Auto start of viewpager
        final Handler handler = new Handler();
        final Runnable Update = new Runnable() {
            public void run() {
                if (currentPage == NUM_PAGES) {
                    currentPage = 0;
                }
                mPager.setCurrentItem(currentPage++, true);
            }
        };
        Timer swipeTimer = new Timer();
        swipeTimer.schedule(new TimerTask() {
            @Override
            public void run() {
                handler.post(Update);
            }
        }, 3000, 3000);

        #Pager listener over indicator
        indicator.setOnPageChangeListener(new ViewPager.OnPageChangeListener() {

            @Override
            public void onPageSelected(int position) {
                currentPage = position;

            }

            @Override
            public void onPageScrolled(int pos, float arg1, int arg2) {

            }

            @Override
            public void onPageScrollStateChanged(int pos) {

            }
        });

    }

}


#activity_main_layout

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="touch.in.keep.sobin.viewpagersample.MainActivity">

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical">
            <RelativeLayout
                android:layout_width="match_parent"
                android:layout_height="250dp"
                android:layout_gravity="top">

                <android.support.v4.view.ViewPager
                    android:id="@+id/pager"
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:layout_alignParentTop="true" />

                <me.relex.circleindicator.CircleIndicator
                    android:id="@+id/indicator"
                    android:layout_width="match_parent"
                    android:layout_height="48dp"
                    android:layout_alignParentBottom="true"
                    android:gravity="bottom" />
            </RelativeLayout>

        </LinearLayout>
    </ScrollView>
</android.support.constraint.ConstraintLayout>

#Custom Adapter for Viewpager

package touch.in.keep.sobin.viewpagersample;

import android.content.Context;
import android.os.Parcelable;
import android.support.v4.view.PagerAdapter;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ImageView;

import java.util.ArrayList;

/**
 * Created by sobin on 17/10/17.
 */

public class SlidingImageAdapter extends PagerAdapter {


    private ArrayList<Integer> IMAGES;
    private LayoutInflater inflater;
    private Context context;


    public SlidingImageAdapter(Context context,ArrayList<Integer> IMAGES) {
        this.context = context;
        this.IMAGES=IMAGES;
        inflater = LayoutInflater.from(context);
    }

    @Override
    public void destroyItem(ViewGroup container, int position, Object object) {
        container.removeView((View) object);
    }

    @Override
    public int getCount() {
        return IMAGES.size();
    }

    @Override
    public Object instantiateItem(ViewGroup view, int position) {
        View imageLayout = inflater.inflate(R.layout.slidinglayout, view, false);

        assert imageLayout != null;
        final ImageView imageView = (ImageView) imageLayout
                .findViewById(R.id.image);


        imageView.setImageResource(IMAGES.get(position));

        view.addView(imageLayout, 0);

        return imageLayout;
    }

    @Override
    public boolean isViewFromObject(View view, Object object) {
        return view.equals(object);
    }

    @Override
    public void restoreState(Parcelable state, ClassLoader loader) {
    }

    @Override
    public Parcelable saveState() {
        return null;
    }


}

#layout for custom Adapter view

<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <ImageView
        android:id="@+id/image"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:adjustViewBounds="true"
        android:layout_gravity="center"
        android:src="@mipmap/ic_launcher"
        android:scaleType="centerCrop" />
</FrameLayout>

#color 
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="colorPrimary">#800080</color>
    <color name="colorPrimaryDark">#800080</color>
    <color name="colorAccent">#8c198c</color>
</resources>

