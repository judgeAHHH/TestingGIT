# Custom code in pWeb applications

### Sends the data gathered from the landing page to the Data Gateway

```javascript
function inviaRichiestaDatagateway(xmlEncoded){
    console.log("invio Richiesta ");
    url= "https://media-platform.doxee.com/DatagatewayProducer/";
	//url = "http://localhost:8080/DatagatewayProducer";
    
    
    postData = {dataXML : xmlEncoded};
    status="false";
    

	//EFFETTUA RICHIESTA
	var jqxhr = $.post( url, postData,"jsonp")
				.done(function(data) {
				  console.log( "success" );
				  console.log(data);
                    if(data.indexOf("OK")> -1){
                        successForm();
                    return true;
                    }
                   
				})
				.fail(function(xhr, textStatus, errorThrown){
				
				  console.log(" Error "+xhr.responseText);
                    $(".messageError").empty();
                    $(".messageError").append('<p style="color:#E60515">Invio non riuscito!</p>');
                    $(".messageError").show();
				  return false;
                   
				  
				})
				.always(function(){
				  console.log( "finished" );
				  
				});
    
    
	 
}
```

### Sends the data gathered from the landing page to the Double Opt-In

```javascript
function inviaRichiesta(xmlEncoded,email,name,surname,codCliente){
    console.log("invio Richiesta ");
    //url= "https://media-platform.doxee.com/DatagatewayProducer/";
	url ="https://media-platform.doxee.com/double_opt_in/iren_welcome/email/";
    
    
    postData = JSON.stringify({"user_data": {"email":email, "name" : name,"surname":surname,"request_id": codCliente+""}, "payload" : xmlEncoded });
    status="false";
    

	//EFFETTUA RICHIESTA
    $.ajax({
  url:url,
  type:"POST",
  data:postData,
  contentType:"application/json; charset=utf-8",
  dataType:"json",
  success: function(data){
    //console.log("success");
	//console.log(data);
      
      if(data.status == "OK"){
           revealForm();
                    return true;
      }else{
          $(".messageError").empty();
          $(".messageError").append('<p style="color:#E60515">Invio non riuscito!</p>');
          $(".messageError").show();
          return false;
      }
	
	/*	else if( data.status == "confirmed"){
			console.log( "success" );
				  console.log(data);
                    
				 
		}
		else if (data.status == "unconfirmed"){
			console.log("non confermato");
		}
        else if (data.status == "not found"){
			console.log("non trovato");
		}
        else if (data.status == "error"){
			console.log("error");
		}
	*/
	
	
	
  },
  error : function(){
      $(".messageError").empty();
          $(".messageError").append('<p style="color:#E60515">Invio non riuscito!</p>');
          $(".messageError").show();
          return false;
  }
        
});	
	return false; 
}	
```

### Template of the XML

```javascript
function getXMLIN(){
	var xml='<?xml version="1.0" encoding="UTF-8" ?><FILE>'+
'<DOCUMENT NOME="" EMAIL="" CELLULARE="" CODICE_CLIENTE="" ACCOUNT="end_u_iren_test" NR_ORDINE="" CODICE_FISCALE="" RID="NO" TIPO_FORM="FORM_EMAIL" GIORNO_SETTIMANA="" FASCIA_ORARIA=""  >'+
'<TEMPLATE AGGIORNAMENTO="F" cod_template="PVIDEO" guid="4a6c8c1fe6274555abda0aa3c4d16734">'+
'</TEMPLATE></DOCUMENT></FILE>';

return xml;
}
```

### Sends the verification email to the user

```javascript
function validateSendEmail(){
    console.log("sendEMailValidation");
    $(".messageError").hide();
    if($("#email").val() != null && $("#email").val() != "" && $("#customCheck1:checked").length >0)
    {
         xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("NOME",JSONobject.NOME);
          if(account != null && account != "")
            xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("ACCOUNT",JSONobject.ACCOUNT);
        xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("EMAIL",$("#email").val());
        xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("CELLULARE",escapeData(JSONobject.CELLULARE));
        xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("CODICE_CLIENTE",escapeData(JSONobject.CODICE_CLIENTE));
        xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("CODICE_FISCALE",escapeData(JSONobject.CODICE_FISCALE));
         var oSerializer = new XMLSerializer();
        var xmlModified = oSerializer.serializeToString(xmlDoc);
        console.log(xmlDoc);
        xmlEncoded= Base64.encode(xmlModified);
        console.log($("#email").val());
       return inviaRichiesta(xmlEncoded,$("#email").val(),name,surname,codiceCliente);
   
    
        
    }
    $(".messageError").empty();
          $(".messageError").append('<p style="color:#E60515">Compilare tutti i campi!</p>');
    $(".messageError").show();
    return false;
}
```

### Checks whether the form is valid or not, if it is it sends the data to the Data Gateway

```javascript
function validateForm(){
        
        if($("#tel").val() != null && $("#tel").val() != "" && $("#customCheckRID:checked").length >0 && 
           ($("#lun:checked").length == 1 || $("#mar:checked").length == 1 || $("#mer:checked").length == 1 || $("#gio:checked").length == 1 || $("#ven:checked").length == 1 || $("#sab:checked").length == 1 || $("#dom:checked").length == 1) && ($("#mattina:checked").length == 1 || $("#pranzo:checked").length == 1 || $("#pom:checked").length == 1)){
            var telefono = $("#tel").val();
            var ggSettimana  = new Array();
            if($("#lun:checked").length == 1)
                ggSettimana.push("Lun");
            if($("#mar:checked").length == 1)
                ggSettimana.push("Mar");
            if($("#mer:checked").length == 1)
                ggSettimana.push("Mer");
            if($("#gio:checked").length == 1)
                ggSettimana.push("Gio");
            if($("#ven:checked").length == 1)
                ggSettimana.push("Ven");
            if($("#sab:checked").length == 1)
                ggSettimana.push("Sab");
            if($("#dom:checked").length == 1)
                ggSettimana.push("Dom");
            var disp="";
            for(i=0;i<ggSettimana.length;i++){
                if(i == ggSettimana.length-1)
                    disp=disp+ggSettimana[i];
                else
                    disp=disp+ggSettimana[i]+"-";
            }
            console.log(disp);
            var fasciaOraria= new Array();
             if($("#mattina:checked").length == 1)
                fasciaOraria.push("Mattina");
            if($("#pranzo:checked").length == 1)
                fasciaOraria.push("Pranzo");
            if($("#pom:checked").length == 1)
                fasciaOraria.push("Pomeriggio");
            var dispFascia="";
            for(i=0;i<fasciaOraria.length;i++){
                if(i == fasciaOraria.length-1)
                    dispFascia=dispFascia+fasciaOraria[i];
                else
                    dispFascia=dispFascia+fasciaOraria[i]+"-";
            }
            
            xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("GIORNO_SETTIMANA",disp);
            xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("CELLULARE",telefono);
            xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("FASCIA_ORARIA",dispFascia);
            xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("TIPO_FORM",escapeData("FORM_RID"));
            xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("RID",escapeData("SI"));
            xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("CODICE_FISCALE",escapeData(JSONobject.CODICE_FISCALE));
             var oSerializer = new XMLSerializer();
        var xmlModified = oSerializer.serializeToString(xmlDoc);
        console.log(xmlDoc);
            console.log(telefono);
        xmlEncoded= Base64.encode(xmlModified);
            
            
            
        return inviaRichiestaDatagateway(xmlEncoded);
        }
        $(".messageError").empty();
          $(".messageError").append('<img src="img/error.png" width="100" /><p style="color:#E60515">Compilare tutti i campi!</p>');
        $(".messageError").show();
        return false;
    }
```

### After the first form is completed, a new one is revealed to the user

```javascript
 function revealForm(){
        $(".messageError").hide();
        $(".message").empty();

        $("#body").hide();

        $("#success-mail").show(400);

        var rid = JSONobject.RID;

        if (rid === "NO") {
            $("#form").show(400);
            setTimeout(function(){

                $("body, html").animate({
                    scrollTop: 280
                }, 1000);

            }, 4000);
        }
    }
```

### Gathers the parameters required from the url

```javascript
var getUrlParameter = function getUrlParameter(sParam) {
		
		// sostituisce i cartatteri speciali nell'url prima di decodificarlo
		var x = window.location.search.substring(1);
		
		var extractIndex=x.indexOf("?",0);
		if(extractIndex != -1)
		x= x.substr(0,extractIndex).trim();
		
		var r = /\\u([\d\w]{4})/gi;
		x = x.replace(r, function (match, grp) {
		return String.fromCharCode(parseInt(grp, 16)); } );
		x = unescape(x);
		
        var sPageURL = decodeURIComponent(x),
            sURLVariables = sPageURL.split('&'),
            sParameterName,
            i;
        for (i = 0; i < sURLVariables.length; i++) {
            sParameterName = sURLVariables[i].split('=');
            if (sParameterName[0] === sParam) {
                return sParameterName[1] === undefined ? true : sParameterName[1];
            }
        }
    };
```

### Initializes the XML Template with data gathered from the landing page

```javascript
//INIZIALIZZA XML TEMPLATE
	var xml = getXMLIN();//RECUPERA XML STANDARD
	var parser, xmlDoc;
	parser = new DOMParser();
	xmlDoc = parser.parseFromString(xml,"text/xml");
    
    console.log(xmlDoc);
    var account = JSONobject.ACCOUNT;
    var email = JSONobject.EMAIL;
    if(email != null)
        $("#email").val(email);
    var name = JSONobject.NOME;
    var surname = JSONobject.COGNOME;
    if(surname == null)
        surname = "Surname";
    var codiceCliente = JSONobject.CODICE_CLIENTE;
    var codiceFiscale = JSONobject.CODICE_FISCALE;
    
    var cellulare =JSONobject.CELLULARE;
    if(cellulare != null){
        $("#tel").val(cellulare);
    }
    
    xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("NOME",JSONobject.NOME);
   // xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("Account",JSONobject.accountName);
   
   if(account != null && account != "")
      xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("ACCOUNT",JSONobject.ACCOUNT);
    
    xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("EMAIL",escapeData(JSONobject.EMAIL));
    xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("CELLULARE",escapeData(JSONobject.CELLULARE));
    xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("CODICE_CLIENTE",escapeData(JSONobject.CODICE_CLIENTE));
    xmlDoc.getElementsByTagName("DOCUMENT")[0].setAttribute("CODICE_FISCALE",escapeData(JSONobject.CODICE_FISCALE));
    
    var oSerializer = new XMLSerializer();
    var xmlModified = oSerializer.serializeToString(xmlDoc);
	console.log(xmlDoc);
	xmlEncoded= Base64.encode(xmlModified);
```