# Custom code in pVideo applications

### Exchange of data between pVideo and Landing Page

It is used to gather up data from the JSON inside the pVideo and to send it to landing page that's opened when a certain action happens

```javascript
/*
	@mailLandingPage contains the landing page url
*/

var mailLandingPage = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].LINK_LP_EMAIL;
if (!this.callToAction_49.hasEventListener("click")){
	this.callToAction_49.actionID = "CONV_01FormMail";
	this.callToAction_49.cursor = "pointer";
	this.callToAction_49.addEventListener("click", callToAction_49ClickHandler);
}

function callToAction_49ClickHandler(evt){
	
	/* 
		These Variables hold required data for the form. We get the data from the JSON inside the pVideo.
		
		@nome holds the name of the user
		@email holds the email of the user
		@cellulare holds the telephone of the user
		@codiceCliente holds the client's code of the user
		@codiceFiscale holds the fiscal code of the user
		@account  
		@rid checks whether the user has a type of payement or not (needed in the landing page)
	*/
	
	var nome = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].NOME;
	var email = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].MAIL_CONTATTO_IREN;
	var cellulare = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].CELLULARE;
	var codiceCliente = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].CODICE_CLIENTE;
	var codiceFiscale = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].CODICE_FISCALE;
	var account = "";
	var rid = "";
	
	/* 
		IF condition that checks if the pvideo is in a TEST or PRODUCTION environment
	*/
	
	if(lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].Ambiente == "TEST")
		account ="end_u_iren_test";
	else
		account ="end_u_iren_prod";
	
	/* 
		IF condition that checks if the user has already a type of payment. Required for the landing page
	*/
	
	if(lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].TIPO_PAGAMENTO == "SDD")
		rid = "SI";
	else
		rid = "NO";
	
	/* 
		@the_data contains the variables initiliazed at the beginning of the function in a JSON format.
		
		Once we have created the object @the_data, we convert it in a JSON variable and then we encode it in Base64.
		After this, we combine the newly created variable with the @mailLandingPage variable  and we open the landing page.
	*/
	
	var the_data = {"NOME":nome,"EMAIL":email,"CELLULARE":cellulare,"CODICE_CLIENTE":codiceCliente,"CODICE_FISCALE":codiceFiscale,"ACCOUNT":account,"QUESTION1": "", "ANSWER1": "","QUESTION2": "", "ANSWER2": "","QUESTION3": "", "ANSWER3": "","QUESTION4": "", "ANSWER4": "",
		"QUESTION5": "", "ANSWER5": "", "RID": rid};
		
	window.open(mailLandingPage + Base64.encode(JSON.stringify(the_data)), "_blank");
	try{
		lib.bus.emit(lib.callToActionClickEvt, evt.currentTarget.actionID);
	} catch(err){
		console.error("Could not sent a call to action identifier. Error:" + err)
	}
}

```

### Exchange between Data Collection and Data Gateway through a Survey inside the pVideo

It's used to collect data gathered from the pVideo. It then sends it to the Data Gateway for processing (?)

```javascript
/*
	@param errorrmsg refers to a error message. It is hidden at the begging of the popup.
*/

this.errormsg.alpha = 0;

/*
	@param chechform1 through @param checkform11 are initialized with the name that it refers to. (???)
*/

this.checkform1.name = "consent1"; //NO Q1
this.checkform2.name = "consent2"; //SI Q1
this.checkform3.name = "consent3"; //NO Q2
this.checkform4.name = "consent4"; //SI Q2
this.checkform5.name = "consent5"; //noabito in condominio Q2
this.checkform6.name = "consent6"; //NO Q4
this.checkform7.name = "consent7"; //SI Q4
this.checkform8.name = "consent8"; //NO Q3
this.checkform9.name = "consent9"; //SI Q3
this.checkform10.name = "consent10"; //NO Q5
this.checkform11.name = "consent11"; //SI Q5

/*
	@param checkforms to contain the names of all checkforms
*/

var checkforms = [];
checkforms.push(this.checkform1);
checkforms.push(this.checkform2);
checkforms.push(this.checkform3);
checkforms.push(this.checkform4);
checkforms.push(this.checkform5);
checkforms.push(this.checkform6);
checkforms.push(this.checkform7);
checkforms.push(this.checkform8);
checkforms.push(this.checkform9);
checkforms.push(this.checkform10);
checkforms.push(this.checkform11);

/*
	@param consents 
*/

var consents = {
	"consent1" : 0,
	"consent2" : 0,
	"consent3" : 0,
	"consent4" : 0,
	"consent5" : 0,
	"consent6" : 0,
	"consent7" : 0,
	"consent8" : 0,
	"consent9" : 0,
	"consent10" : 0,
	"consent11" : 0
};

/*
	@param consentChoice contains the changes made through the checkforms.
*/

var consentChoice = [0,0,0,0,0,0,0,0,0,0,0];

/*
	Serve a modificare il consent presente nel json?
	Serve a indicare cosa viene modificato?
	Serve a renderizzare la X?
*/

function renderConsents(consentChoice, checkforms){
	var dataSchemaRoot = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0];
	
	for (var i = 0; i<consentChoice.length; i++){
		if(checkforms[i].name == "consent" + (i+1)){
			dataSchemaRoot[checkforms[i].name] = consentChoice[i];
			console.log("renderConsents after:", checkforms[i].name + " - " + consentChoice[i]);
		}
	}
}

/*
	IF condition that adds event listeners to the checkforms and changes the cursor appearence when it goes over a checkform
*/


if (!this.checkform1.hasEventListener("click")) {
	this.checkform1.addEventListener("click", toggleChecked.bind(this));
	this.checkform2.addEventListener("click", toggleChecked.bind(this));
	this.checkform3.addEventListener("click", toggleChecked.bind(this));
	this.checkform4.addEventListener("click", toggleChecked.bind(this));
	this.checkform5.addEventListener("click", toggleChecked.bind(this));
	this.checkform6.addEventListener("click", toggleChecked.bind(this));
	this.checkform7.addEventListener("click", toggleChecked.bind(this));
	this.checkform8.addEventListener("click", toggleChecked.bind(this));
	this.checkform9.addEventListener("click", toggleChecked.bind(this));
	this.checkform10.addEventListener("click", toggleChecked.bind(this));
	this.checkform11.addEventListener("click", toggleChecked.bind(this));
	
	this.checkform1.cursor = "pointer";
	this.checkform2.cursor = "pointer";
	this.checkform3.cursor = "pointer";
	this.checkform4.cursor = "pointer";
	this.checkform5.cursor = "pointer";
	this.checkform6.cursor = "pointer";
	this.checkform7.cursor = "pointer";
	this.checkform8.cursor = "pointer";
	this.checkform9.cursor = "pointer";
	this.checkform10.cursor = "pointer";
	this.checkform11.cursor = "pointer";
	
}

/*
	IF condition that adds an event listerner to the confirmform and change the cursor appearence when it goes over the button
*/

if (!this.Confirmform.hasEventListener("click")) {
	this.Confirmform.addEventListener("click", sendConfirmation.bind(this));
	this.Confirmform.cursor = "pointer";
}

/*
	@func toogleChecked checks which checkform has been clicked.
*/

function toggleChecked(event)
{
	/*
		the code inside the try and catch is needed to understand which of the checkforms has been clicked
	*/
	
	try {
		if(event.currentTarget.currentLabel != null)
		{
			if(event.currentTarget.currentLabel == "normal")
			{
				event.currentTarget.gotoAndStop("checked");
				
				//QUESTION1
				if(event.currentTarget.name == checkforms[0].name)
				{
					checkforms[1].gotoAndStop("normal");
					consentChoice[1] = 0;
				}
				else if(event.currentTarget.name == checkforms[1].name)
				{
					checkforms[0].gotoAndStop("normal");
					consentChoice[0] = 0;
				}
				//QUESTION2
				else if(event.currentTarget.name == checkforms[2].name)
				{
					checkforms[3].gotoAndStop("normal");
					consentChoice[3] = 0;
					checkforms[4].gotoAndStop("normal");
					consentChoice[4] = 0;
				}
				else if(event.currentTarget.name == checkforms[3].name)
				{
					checkforms[2].gotoAndStop("normal");
					consentChoice[2] = 0;
					checkforms[4].gotoAndStop("normal");
					consentChoice[4] = 0;
				}
				else if(event.currentTarget.name == checkforms[4].name){
					checkforms[2].gotoAndStop("normal");
					consentChoice[2] = 0;
					checkforms[3].gotoAndStop("normal");
					consentChoice[3] = 0;
				}
				//QUESTION4
				else if(event.currentTarget.name == checkforms[5].name)
				{
					checkforms[6].gotoAndStop("normal");
					consentChoice[6] = 0;
				}
				else if(event.currentTarget.name == checkforms[6].name)
				{
					checkforms[5].gotoAndStop("normal");
					consentChoice[5] = 0;
				}
				//QUESTION3
				else if(event.currentTarget.name == checkforms[7].name)
				{
					checkforms[8].gotoAndStop("normal");
					consentChoice[8] = 0;
				}
				else if(event.currentTarget.name == checkforms[8].name)
				{
					checkforms[7].gotoAndStop("normal");
					consentChoice[7] = 0;
				}
				//QUESTION5
				else if(event.currentTarget.name == checkforms[9].name)
				{
					checkforms[10].gotoAndStop("normal");
					consentChoice[10] = 0;
				}
				else if(event.currentTarget.name == checkforms[10].name)
				{
					checkforms[9].gotoAndStop("normal");
					consentChoice[9] = 0;
				}
					
			} else
			{
				event.currentTarget.gotoAndStop("normal");
				
				
			}
		}
	} catch(error) {
		console.error(error);
	}
	
	/*
		IF condition that checks which checkform has been clicked.
	*/
	
	for (var i = 0; i < checkforms.length; i++){
		if(event.currentTarget.name == checkforms[i].name){
			if(event.currentTarget.currentLabel == "normal")
			{
				consentChoice[i] = 0;
			} 
			else 
			{
				consentChoice[i] = 1;
			}
			 
			if(event.currentTarget.name == "consent5" && event.currentTarget.currentLabel == "checked"){
				
				consentChoice[i] = 2;
				}
		}
	}
	
	renderConsents(consentChoice, checkforms);
}

/*
	@func sendConfimation gathers up the data from pVideo and sends it to the Data Gateway/Landing page
*/

function sendConfirmation(){
	// question 1
	
	/*
		Gathers up the responses from survey, from the JSON in the pVideo
		@param q1 gathers the question 
		@param consent1 gathers the response
		@param consent2 gathers the response
		@paran a1 gathers the answer from @param consent1 or @param consent2
	*/
	var q1 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].QUESTION01;

	var consent1 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent1;
	var consent2 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent2;
	var a1 = "";
	
	/*
		IF condition that checks which checkform has been checked and gives the answer to @param a1
	*/
	
	if (consent1  == 0 && consent2  == 0)
		a1 = "";
	else if (consent2 == 0)
		a1 = "NO";
	else
		a1 = "SI";
	
	// question 2
	/*
		@param q2 gathers the question 
		@param consent3 gathers the response
		@param consent4 gathers the response
		@param consent5 gathers the response
		@paran a2 gathers the answer from @param consent3 or @param consent4 or @param consent5
	*/
	
	var q2 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].QUESTION02;
	var a2 = "";
	var consent3 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent3;
	var consent4 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent4;
	var consent5 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent5;

	/*
		IF condition that checks which checkform has been checked and gives the answer to @param a1
	*/
	
	if (consent3 == 0 && consent4 == 0 && consent5 == 0)
		a2 = "";
	else if (consent5 == 0)
		if (consent4 == 0)
			a2 = "NO";
		else
			a2 = "SI";
	else
		a2 = "No, abito in condominio"; 


	// question 3
	/*
		@param q3 gathers the question 
		@param consent8 gathers the response
		@param consent9 gathers the response
		@paran a3 gathers the answer from @param consent8 or @param consent9
	*/	
		
	var q3 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].QUESTION03;
	var a3 = "";
	var consent8 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent8;
	var consent9 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent9;
	
	/*
		IF condition that checks which checkform has been checked and gives the answer to @param a1
	*/
	
	if (consent8 == 0 && consent9 == 0)
		a3  = "";
	else if (consent9 == 0)
		a3  = "NO";
	else
		a3 = "SI";
	
	
	// question 4
	/*
		@param q4 gathers the question 
		@param consent6 gathers the response
		@param consent7 gathers the response
		@paran a4 gathers the answer from @param consent6 or @param consent7
	*/		
	
	var q4 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].QUESTION04;
	var consent6 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent6;
	var consent7 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent7;
	
	var a4 = "";

	/*
		IF condition that checks which checkform has been checked and gives the answer to @param a1
	*/
	
	if (consent6 == 0 && consent7 == 0)
		a4  = "";
	else if (consent7 == 0)
		a4  = "NO";
	else
		a4  = "SI";
	
	
	// question 5
	/*
		@param q5 gathers the question 
		@param consent10 gathers the response
		@param consent11 gathers the response
		@paran a4 gathers the answer from @param consent10 or @param consent11
	*/		
	
	
	var q5 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].QUESTION05;
	
	var a5 = "";
	var consent10 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent10;
	var consent11 = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent11;
	
	/*
		IF condition that checks which checkform has been checked and gives the answer to @param a1
	*/
	
	if (consent10 == 0 && consent11 == 0)
		a5  = "";
	else if (consent11 == 0)
		a5  = "NO";
	else
		a5 = "SI";
	
	// other data
	/*
		Data needed for the exchange between the pVideo and the landing page
		@param nome holds the name of the user
       		@param email holds the email of the user
        	@param cellulare holds the telephone of the user
        	@param codiceCliente holds the client's code of the user
        	@param codiceFiscale holds the fiscal code of the user
        	@param account
	*/
	
	var nome = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].NOME;
	var email = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].MAIL_CONTATTO_IREN;
	var cellulare = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].CELLULARE;
	var codiceCliente = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].CODICE_CLIENTE;
	var codiceFiscale = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].CODICE_FISCALE;
	var account = "";
	
	/* 
        	IF condition that checks if the pvideo is in a TEST or PRODUCTION environment
    	*/	
	
	if(lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].Ambiente == "TEST")
		account ="end_u_iren_test";
	else
		account ="end_u_iren_prod";
	
	/*
		@param the_data gathers up the data from the pVideo and puts it in a JSON format
	*/
	var the_data = {"NOME":nome,"EMAIL":email,"CELLULARE":cellulare,"CODICE_CLIENTE":codiceCliente,"CODICE_FISCALE":codiceFiscale,"ACCOUNT":account,"QUESTION1": q1, "ANSWER1": a1,"QUESTION2": q2, "ANSWER2": a2,"QUESTION3": q3, "ANSWER3": a3,"QUESTION4": q4, "ANSWER4": a4,
		"QUESTION5": q5, "ANSWER5": a5};
	console.log(the_data);
	
	/*var c1 = (lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent1 == 0) ? "NO" : "SI";
	var c2 = (lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent2 == 0) ? "NO" : "SI";
	var c3 = (lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent3 == 0) ? "NO" : "SI";

	console.log("sendConfirmation1: ", c1);
	console.log("sendConfirmation2: ", c2);
	console.log("sendConfirmation3: ", c3);
	
	var the_data = {"first_name":lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].first_name,
					"consent1":c1,
					"consent2":c2,
					"consent3":c3,
					"consent1_title":lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent1_title,
					"consent2_title":lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent2_title,
					"consent3_title":lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].consent3_title
					};
	      
	this.Confirmform.gotoAndStop("done");
	this.Confirmform.removeAllEventListeners();
	this.Confirmform.cursor = "normal";*/
	
	/*
		@param link_survey contains the url to the landing page
	*/
	
	var link_survey = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].LINK_LP_SURVEY;
		
	/*
		IF condition that checks whether a checkform has been checked. If it hasn't, it displays an error immage. If not, we convert @param the_data in a JSON variable and then we encode it in Base64. After this, we combine the variable with @param link_survey and we open the landing page.
	*/
	
	if (consent1 == 0 && consent2 == 0 && consent3 == 0 && consent4 == 0 && consent5 == 0 && consent6 == 0 && consent7 == 0 && consent8 == 0 && consent9 == 0 && consent10 == 0 && consent11 == 0){
		this.errormsg.alpha = 1;
		return;
	} else {
		window.open(link_survey + Base64.encode(JSON.stringify(the_data)), "_blank");
		this.errormsg.alpha = 0;
		this.Confirmform.removeAllEventListeners();
		this.Confirmform.cursor = "normal";	
		return;
	}
	
	
	/* window.open(link_survey + Base64.encode(JSON.stringify(the_data)), "_blank"); */
}
```
