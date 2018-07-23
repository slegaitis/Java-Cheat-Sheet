# Java-Cheat-Sheet
Java cheat sheet, helpful functions etc
Java Usefull things:

Set portrait mode for entire application.

    public class AppName extends Application {
            @Override
            public void onCreate() {
                super.onCreate();
                registerActivityLifecycleCallbacks(new ActivityLifecycleCallbacks() {
                    @Override
                    public void onActivityCreated(Activity activity, Bundle savedInstanceState) {
                        activity.setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);
                    }
                }
            }
        }
        
Then in AndroidManifest.xml under application you need to add 

    android:name=".AppName"

**************************************************

Navigation with Stack:
    import java.util.Stack;

    private Stack<Integer> pageStack = new Stack<Integer>();

push() - Adds new page to a stack && pop() - Removes page from a stack

**************************************************

Logging, get activity tag:

    public static final String TAG = StoryActivity.class.getSimpleName();

**************************************************
Remove action bar, go to res -> values -> styles.xml: 

    style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar"

**************************************************

HTTP call:

        String apiKey = "6e9b6fdc47d3747796758f08c99843bb";
        double latitude = 37.8267;
        double longtitude = -122.4233;
        String forecastURL = "https://api.darksky.net/forecast/"
                + apiKey + "/"
                + latitude + ","
                + longtitude;

        OkHttpClient client = new OkHttpClient();
        Request request = new Request.Builder()
                .url(forecastURL)
                .build();
        Call call = client.newCall(request);
        call.enqueue(new Callback() {
            @Override
            public void onFailure(Call call, IOException e) {
                Log.e(TAG, "IO Exception caught: ", e);
            }

            @Override
            public void onResponse(Call call, Response response) throws IOException {
                try {
                    if(response.isSuccessful()){
                        Log.v(TAG, response.body().string());
                    }
                } catch(IOException e) {
                    Log.e(TAG, "IO Exception caught: ", e);
                }
            }
        });

***************************************************

Check network availability:

    private boolean isNetworkAvailable() {
            ConnectivityManager manager = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
            NetworkInfo networkInfo = manager.getActiveNetworkInfo();
            Boolean isAvailable = false;

            if(networkInfo != null && networkInfo.isConnected()) {
                isAvailable = true;
            }

            return isAvailable;
        }
        
        
***************************************************

Data binding:

In gradle file add:

    android{
       ....
       dataBinding {
           enabled = true;
       }
    }

Bind data from a class. In activity xml file (e.g activity_main.xml) wrap android.support.constraint.ConstraintLayout with 

    <layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools">
        <data>
            <variable
                name="weather"
                type="com.danskebank.stormy.CurrentWeather"/>
        </data>
        
    </layout>

type="" to be equal to a class you want to bind data from and name="" to equal a variable you want to assign. Make sure the class from which you want to bind values have constructors. 1 with no parameters and 2nd with all parameters within the class.

Usage:
To use this in android:text="" add @{variable_name.value}

    <TextView
            android:id="@+id/summaryValue"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginBottom="8dp"
            android:layout_marginEnd="8dp"
            android:layout_marginLeft="8dp"
            android:layout_marginRight="8dp"
            android:layout_marginStart="8dp"
            android:layout_marginTop="15dp"
            android:text="@{weather.summary}"
            android:textColor="@android:color/white"
            android:textSize="18sp"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/precipValue" />
            
 Finally update Activity setContentView() in onCreate method to:

     ActivityMainBinding binding = DataBindingUtil.setContentView(MainActivity.this, R.layout.activity_main);
     
 Things to note, data binding class is created after 1st build, ActivityMainBinding class is created if your xml file is name activity_main.xml if its other name e.g main_activity.xml binding class will be called MainActivityBinding. Concatinate strings with data binding: android:text="@{`At ` + String.valueOf(weather.formatedTime) + ` it will be`}"
 
 
 To bind data:
 
 Get all variables that will be used within that activity and pass to binding variable we created above:
 
     CurrentWeather displayWeather = new CurrentWeather(
            currentWeather.getLocationLabel(),
            currentWeather.getIcon(),
            currentWeather.getTime(),
            currentWeather.getTemperature(),
            currentWeather.getHumidity(),
            currentWeather.getPrecipChance(),
            currentWeather.getSummary(),
            currentWeather.getTimezone());

    binding.setWeather(displayWeather);
