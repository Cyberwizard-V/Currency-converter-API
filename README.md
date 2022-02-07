# Currency-converter-API
A Currency converter in java
-----------------------------------------

It's the .Class file in src. :) 

- Dutch


----------------------------------------- RAW CODE -----------------------------------------

```java
import org.json.JSONObject;

import javax.net.ssl.HttpsURLConnection;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.HashMap;
import java.util.Scanner;

public class CurrencyConverter {

    public static void main(String[] args) throws IOException {

        HashMap<Integer, String> currencyCodes = new HashMap<Integer, String>();

        // CurrencyCodes toevoegen
        currencyCodes.put(1, "USD");
        currencyCodes.put(2, "CAD");
        currencyCodes.put(3, "EUR");
        currencyCodes.put(4, "HKD");
        currencyCodes.put(5, "INR");


        String fromCode, toCode;
        double amount;

        Scanner sc = new Scanner(System.in);

        System.out.println("Welkom bij Viktor's Currency Converter !");

        System.out.println("Je gaat EUR omzetten naar");
        System.out.println("3:EUR ( Je hebt alleen 3 )");
        fromCode = currencyCodes.get(sc.nextInt());

        System.out.println("Naar welke Currency wil je " + fromCode + " Omzetten");
        System.out.println("1:USD \t 2:CAD \t 3:EUR \t 4:HKD \t 5:INR");
        toCode = currencyCodes.get(sc.nextInt());

        System.out.println("Bedrag dat u je wilt omzetten");
        amount = sc.nextFloat();

        sendHttpGETRequest(toCode, amount);

        System.out.println("Bedankt voor het gebruiken van mijn converter !");
    }

    private static void sendHttpGETRequest(String toCode, double amount) throws IOException {


        //http://api.exchangeratesapi.io/v1/latest?access_key=1320a179ce5dc1d0de4483b11054e505&format=1

        String API_KEY  = "?access_key=1320a179ce5dc1d0de4483b11054e505";

        String GET_URL = "http://api.exchangeratesapi.io/v1/latest" + API_KEY + "&base=EUR" + "&symbols=" + toCode;
        URL url = new URL(GET_URL);
        HttpURLConnection httpsURLconnection = (HttpURLConnection) url.openConnection();
        httpsURLconnection.setRequestMethod("GET");
        int responseCode = httpsURLconnection.getResponseCode();


        if(responseCode == HttpURLConnection.HTTP_OK) { //hier kan ik zien of mijn get request succesvol is.
            //hiervoor gebruik ik een buffered reader
            BufferedReader in = new BufferedReader(new InputStreamReader(httpsURLconnection.getInputStream()));
            String inputLine;
            StringBuilder response = new StringBuilder();


            while ((inputLine = in.readLine()) != null) {
                response.append(inputLine);
            }
            in.close();

            String baseEUR = "EUR";
            //alles komt in een json format. ( data van de API )
            //System.out.println(response);



            JSONObject obj = new JSONObject(response.toString());
                                                                                    //String ValutaWaarde = obj.getString("base");
                                                                                    //System.out.println(obj.getJSONObject("rates"));
            Double RATE = obj.getJSONObject("rates").getDouble(toCode);
                                                                                    //System.out.println(RATE);
                                                                                    //System.out.println(ValutaWaarde); // testing
            Double Convert = amount * RATE;
            System.out.println(Convert);
            System.out.println(amount + " " + baseEUR + " = " + Convert + toCode);
        } else {
            System.out.println(responseCode);
            System.out.println("get request FOUT");


        }
    }
}
```
