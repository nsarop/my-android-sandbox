```java

//this activity fetches company dtails from an online mySQL database
package com.nsaro.www.mobiledirectory;

import android.app.ProgressDialog;
import android.content.Context;
import android.net.ConnectivityManager;
import android.net.NetworkInfo;
import android.os.AsyncTask;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import org.apache.http.HttpResponse;
import org.apache.http.NameValuePair;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.message.BasicNameValuePair;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.ArrayList;

public class companydetailsActivity extends AppCompatActivity {
 String companyname_selected;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_companydetails);

        Bundle mybundle=getIntent().getExtras();
        companyname_selected=mybundle.getString("company name selected");

        if(haveNetworkConnection()){
            new Loadcompanydetails().execute();
        } else {
            Toast.makeText(this, "No internet connection, try again", Toast.LENGTH_SHORT).show();
        }
    }


    class Loadcompanydetails extends AsyncTask<String,String,String>{
              ProgressDialog dialog;
              String php_response;
        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            dialog=new ProgressDialog(companydetailsActivity.this);
            dialog.setMessage("Loading Details..");
            dialog.setCancelable(false);
            dialog.setIndeterminate(false);
            dialog.show();
        }

        @Override
        protected String doInBackground(String... strings) {

            try {
                DefaultHttpClient httpclient = new DefaultHttpClient();

                HttpPost httppost = new HttpPost("http://mbinitiative.com/mobiledirectory_nsaro/getcompanydetails.php");
                // Add your data
                ArrayList<NameValuePair> nameValuePairs = new ArrayList<NameValuePair>(1);
                nameValuePairs.add(new BasicNameValuePair("companyname",companyname_selected));
                httppost.setEntity(new UrlEncodedFormEntity(nameValuePairs));
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
            dialog.dismiss();
           // Toast.makeText(companydetailsActivity.this, "data received"+php_response, Toast.LENGTH_SHORT).show();

       String[] details=php_response.split("#");

            TextView textViewName=findViewById(R.id.textViewcname);
            TextView textViewRegion=findViewById(R.id.textViewcregion);
            TextView textViewCategory=findViewById(R.id.textViewccategory);
            TextView textViewaddress=findViewById(R.id.textViewpaddress);
            TextView textViewbiography=findViewById(R.id.textViewcbio);
            Button buttonPhone= findViewById(R.id.buttoncphone);
            Button buttonEmail=findViewById(R.id.buttoncemail);

            textViewName.setText(details[0]);
            textViewCategory.setText("category :" + details[5]);
            textViewRegion.setText("Region :" +details[6]);
            textViewaddress.setText("Address :" +details[3]);
            textViewbiography.setText(details[4]);
            buttonPhone.setText(details[1]);
            buttonEmail.setText(details[2]);
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
