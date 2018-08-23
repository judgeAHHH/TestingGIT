# Custom code in pVideo applications

This a file in which we explain every custom code contained in the various pVideo and pWeb applications. You can also re-utilize the code featured in here in other projects.

### IREN

Costum code found in "Scene 20", "CallToActionSnippet" layer:

```javascript
/*
	@mailLandingPage contains an url that links to the web part of the applcation
*/
var mailLandingPage = lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].LINK_LP_EMAIL;
if (!this.callToAction_49.hasEventListener("click")){
	this.callToAction_49.actionID = "CONV_01FormMail";
	this.callToAction_49.cursor = "pointer";
	this.callToAction_49.addEventListener("click", callToAction_49ClickHandler);
}
function callToAction_49ClickHandler(evt){
	
	/* 
		These Variables hold required data for the form. We get the data from the JSON inside the pvideo.
		
		@nome holds the name of the user
		@email holds the email of the user
		@cellulare holds the telephone of the user
		@codiceCliente holds the client's code of the user
		@codiceFiscale holds the fiscal code of the user
		@account  
		@rid checks whether the user has a type of payement or not (needed in the web part of the application)
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
		IF condition that checks if the user has already a type of payment. This part is required for a condition that we show 
		in the web part of the application 
	*/
	
	if(lib.injectedData.docData.FILE.DOCUMENT[0].TEMPLATE[0].CUSTOMER_DATA[0].TIPO_PAGAMENTO == "SDD")
		rid = "SI";
	else
		rid = "NO";
	
	/* 
		@the_data contains the variables initiliazed at the beginning of the function in a JSON format.
		
		Once we have created the object @the_data, we convert it in a JSON variable and then we encode it in Base64.
		After this, we combine the newly created variable with the @mailLandingPage variable (which contains an url) and 
		we open a new window in the browser.
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

### Installing

A step by step series of examples that tell you how to get a development env running

Say what the step will be

```
Give the example
```

And repeat

```
until finished
```

End with an example of getting some data out of the system or using it for a little demo

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc
