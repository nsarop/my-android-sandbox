```java


package com.nsaro.www.mobiledirectory;

import android.app.ProgressDialog;
import android.content.Context;
import android.content.Intent;
import android.net.ConnectivityManager;
import android.net.NetworkInfo;
import android.os.AsyncTask;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;
import android.widget.Toast;

import org.apache.http.HttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.DefaultHttpClient;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;

public class viewcompaniesActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_viewcompanies);

        if (haveNetworkConnection()){
            new loadcampanies().execute();
        } else{
            Toast.makeText(this, "No internet connection", Toast.LENGTH_SHORT).show();
        }
    }


    class loadcampanies extends AsyncTask<String,String,String>{

        ProgressDialog pdialog;
        String php_response;

        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            pdialog=new ProgressDialog(viewcompaniesActivity.this);
            pdialog.setMessage("Loading");
            pdialog.setCancelable(false);
            pdialog.setIndeterminate(false);
            pdialog.show();
        }

        @Override
        protected String doInBackground(String... strings) {

            try {
                DefaultHttpClient httpclient = new DefaultHttpClient();

                HttpPost httppost = new HttpPost("http://mbinitiative.com/mobiledirectory_nsaro/getcompanies.php");

                // Execute HTTP Post Request
                HttpResponse response = httpclient.execute(httppost);

                InputStream inputStream = response.getEntity().getContent();

                BufferedReader rd = new BufferedReader(new InputStreamReader(
                        inputStream), 4096);
                String line;
                StringBuilder sb = new StringBuilder();

                while ((line = rd.readLine()) != null) {
                    sb.append(line);
                }
                rd.close();
                php_response = sb.toString();

                inputStream.close();

            } catch (Exception e) {
                Toast.makeText(getApplicationContext(),
                        "Error inside set:" + e.toString(), Toast.LENGTH_LONG)
                        .show();
            }

            return php_response;

        }

        @Override
        protected void onPostExecute(String s) {
            super.onPostExecute(s);
            pdialog.dismiss();
           // Toast.makeText(viewcompaniesActivity.this, "Data received" +php_response, Toast.LENGTH_SHORT).show();
       //create list of companies
            final String[] company_name=php_response.split("#");
            ListView listViewcompanies=findViewById(R.id.listviewcompanies);
            ArrayAdapter<String> adapter=new ArrayAdapter<String>(
                    viewcompaniesActivity.this,
                    R.layout.listitemdesign,
                    R.id.textViewlistitem,
                    company_name);
            listViewcompanies.setAdapter(adapter);
            listViewcompanies.setOnItemClickListener(new AdapterView.OnItemClickListener() {
                @Override
                public void onItemClick(AdapterView<?> parent, View view, int position, long id) {

                    //capture selected company name
                    String selectname= company_name[position];
                    Intent viewintent= new Intent(viewcompaniesActivity.this,companydetailsActivity.class);
                   Bundle mybundle=new Bundle();
                   mybundle.putString("company name selected",selectname);
                    viewintent.putExtras(mybundle);
                    startActivity(viewintent);
                }
            });




        }
    }

     //method to check internet availability(WiFi and MobileData)
         private boolean haveNetworkConnection() {
             boolean haveConnectedWifi = false;
             boolean haveConnectedMobile = false;

             ConnectivityManager cm = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
             NetworkInfo[] netInfo = cm.getAllNetworkInfo();
             for (NetworkInfo ni : netInfo) {
                 if (ni.getTypeName().equalsIgnoreCase("WIFI"))
                     if (ni.isConnected())
                         haveConnectedWifi = true;
                 if (ni.getTypeName().equalsIgnoreCase("MOBILE"))
                     if (ni.isConnected())
                         haveConnectedMobile = true;
             }
             return haveConnectedWifi || haveConnectedMobile;
         }
}
