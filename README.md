1. Create a sandbox account from:
		[[https://developer.sslcommerz.com/registration/]] 
You can put any dummy data for registration, except your email address. 
[Note: Email address should be valid. You will receive a email verification code through your email. Put the information that you will remember].

2. Click here to login to the merchant panel:
	[[https://sandbox.sslcommerz.com/manage/]]
	Login ID: "That you have given during the registration"
	Password: "That you have given during the registration"
[Note: Now you will get an email containing with STORE_ID and STORE_PASSWORD, it might take a couple of seconds]

3. Go to .env file from your laravel project:
	Add those following lines at the end of the file:
		STORE_ID = That you received from the mail
		STORE_PASSWORD = That you received from the mail
 Set your database name, username, and password at the same time. 
	

4. Download the SslCommerz zip file or clone in any folder:
	[[https://github.com/sslcommerz/SSLCommerz-Laravel]]
 	The file folder structure look like this,
		 |-- config/
    			|-- sslcommerz.php
		 |-- app/Library/SslCommerz
    			|-- AbstractSslCommerz.php (core file)
    			|-- SslCommerzInterface.php (core file)
    			|-- SslCommerzNotification.php (core file)
 		|-- README.md
 		|-- orders.sql (sample)

5. Now follow the steps to integrate payment gateway to integrate to your paypal project:  
		Step 1: Extract the file
		Step 2: Copy the Library folder and put it in the laravel 		project's app/ directory. If needed, then run composer dump -		o. 
		Step 3: Copy the config/sslcommerz.php file into your laravel project's config/ folder.
		Step 4: Copy the SslCommerzPaymentController into your laravel project's Controllers folder
		Step 5: Copy the defined routes from routes/web.php into your laravel project's route file.
		Step 6: Add the below routes into the $excepts array of VerifyCsrfToken middleware.

			protected $except = [
    				'/pay-via-ajax', '/success','/cancel','/fail','/ipn'
			];

		Step 7: Copy all the view files under the resource folder and paste into your laravel project's resources/views/ folder.
		Step 8: To integrate popup checkout, use the below script before the end of body tag. Add it in any blade template that you want add your button. 
			For Example: welcome.blade.php-> paste the code below, before ending of the body tag.
		
		For Sandbox:

			<script>
   				 (function (window, document) {
        				var loader = function () {
            				var script = document.createElement("script"), tag = document.getElementsByTagName("script")[0];
           				script.src = "https://sandbox.sslcommerz.com/embed.min.js?" + Math.random().toString(36).substring(7);
            				tag.parentNode.insertBefore(script, tag);
        			};

       				 	window.addEventListener ? window.addEventListener("load", loader, false) : window.attachEvent("onload", loader);
    				})(window, document);
			</script>

		For Live:

			<script>
    				(function (window, document) {
        				var loader = function () {
           				var script = document.createElement("script"), tag = document.getElementsByTagName("script")[0];
            				script.src = "https://seamless-epay.sslcommerz.com/embed.min.js?" + Math.random().toString(36).substring(7);
            				tag.parentNode.insertBefore(script, tag);
        			};
    
        				window.addEventListener ? window.addEventListener("load", loader, false) : window.attachEvent("onload", loader);
    				})(window, document);
			</script>

		Step 9: Use the below button where you want to show the "Pay Now" button:
			Should be in the same place where step 8 was implemented. 

			<button class="your-button-class" id="sslczPayBtn"
        			token="if you have any token validation"
        			postdata="your javascript arrays or objects which requires in backend"
        			order="If you already have the transaction generated for current order"
        			endpoint="/pay-via-ajax"> Pay Now
			</button>
6. There is a orders.sql file in the SslCommerz folder. You can import it directrly from the phpmyadmin. Or you make a migration and model manually. And create new table according to the oraders.sql file columns. 

7. You're done!




Some problem you might face.
Possible solution:-
	-> Check DB class in the SslCommerzPaymentController and change, use DB into Illuminate\Support\Facades\DB;
	-> Import SslCommerzPaymentController from the web.php.
	-> Migrate the table.
	-> Add this line of code to the payViaAjax method in the SslCommerzPaymentController, if you want to retrieve data dynamically from the form

 		public function payViaAjax(Request $request, $id)
   				 {
					<<$requestData = (array)json_decode($request->cart_json);>>


