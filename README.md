# fyers-java

   *fyers-java is an open source Java library that can talk to [Fyers](https://fyers.in/) trading endpoint.*

### How to use it?

#### 1. Download and build the code

		git clone https://github.com/theselfspace/fyers-java
		mvn clean install

#### 2. Add following dependency in your project's pom.xml

        <dependency>
			<groupId>space.theself</groupId>
			<artifactId>fyers-java</artifactId>
			<version>1.0.0</version>
        </dependency>

#### 3. Login to Fyers 
Login is done by `LoginHandler` class that needs app id and redirect URL, you can get this from Fyers app configuration portal.

		LoginHandler loginHandler = new LoginHandler();
		loginHandler.setClientCode("<YOUR APP ID>");
		loginHandler.setRedirectUrl("<YOUR APP SPECIFIC REDIRECT URL>");
		
Once you provide that, you can call the `login` method that requires parameters similar to what you would need to login Fyers via web:
		
		String authCode = loginHandler.login("<LOGIN USERNAME>", "<LOGIN PASSWORD>", "<4 DIGIT MFA>");

Once it generates the auth code successfully, you can now trigger the API calls.

#### 4. Trigger API
API calls are handled by `FyersFly` class. `APP SECRET` is generated by the Fyers API portal when the app is created.

		FyersFly fly = new FyersFly("<YOUR APP ID>", "<APP SECRET>");

Now using the `authCode` that was generated by `LoginHandler` we can generate the access token.

		String token = fly.generateAccessToken(authCode);
		fly.setAccessToken(token);

After this, we can start calling the available APIs

		System.out.println("GET PROFILE");
		Profile profile = fly.getProfile();
		System.out.println(gson.toJson(profile));

	 	System.out.println("GET HOLDING");
		Holding[] holdings = fly.getHolding();
		System.out.println(gson.toJson(holdings));

		System.out.println(gson.toJson(fly.getMarketDepth("NSE:SBIN-EQ", 1)));
		
Please be careful while placing a buy or sell order as it requires instrument name in a specific format:
For buy, the format is `<EXCHANGE>:<INSTRUMENT>`

		fly.placeBuyOrder("NSE:NIFTY2262315300PE", 50, FyerParam.ORDER_TYPE_MARKET, FyerParam.ORDER_PRODUCTTYPE_INTRADAY);
		
For sell, the format is `<EXCHANGE>:<INSTRUMENT>-<PRODUCT TYPE>`

		fly.placeExitPositionOrderById("NSE:NIFTY2262315300PE-INTRADAY");				
		

## PLEASE NOTE
**

** 1. Please read the code or test case first to know the pre-requisite before using any command **

** 2. The code is written for a specific purpose and can further be generalized. Especially Login flow needs to be followed as per current implementation. Login flow is quite similar to how is traversed through web portal (i.e. login via user and password and provide 4 digit pre-defined otp) **

** 3. Logout flow still requires some more exploration **

** 4. Commands that are related to EDIS are not tested well, please test it before using those commands. **

** 5. Read the [official docs](https://myapi.fyers.in/docs/) very well! Sometimes you might see a failure in API response but it MIGHT have actually got executed successfully. **

**

For any queries, please write me at <theself.space@gmail.com> or ping at [Theself_Space](https://twitter.com/Theself_Space)

