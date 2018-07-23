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
