---
layout:     post
title:      Testing framework 
subtitle:   Testing framework 
date:       2018-02-04
author:     BY Jeff Zhan
header-img: img/post-bg-BJJ.jpg
catalog: true
tags:
    - Test
---
Jeff zhan.

{% plantuml %}

skinparam class {
	BackgroundColor PaleGreen
	ArrowColor SeaGreen
	BorderColor SpringGreen
}

skinparam object {
	BackgroundColor Yellow
	ArrowColor SeaGreen
	BorderColor SpringGreen
}

Package Bussiness {

class Esim  << (T,trait) >>{
+sendGetAuthenticationOnly()

}

class GenericDevice << (T,trait) >>{

 +sendAuthorizeDevceRequest()

}

class CustomerCare << (T,trait) >>{
  +sendCustomerCareRequest()
}

class Oauth << (T,trait) >>{
  +sendGenerateCodeRequest()
}

}




Package Simulator {

object MocoServer {
+startupWithOutFile()
}
 object ConfirmOrderMoco {
  +getAllMoco()
  +verifyMocoRequest()
 }
 object ThirdPartyMoco{
   +getAllMoco()
  }

  object TrapServer{
   +start()
   +waitAndGetAlarm()
   +verifyAlarms()
  }

  ConfirmOrderMoco <--o ThirdPartyMoco
  MocoServer --> ThirdPartyMoco
}

Package TestCaseBasic {
class FeatureSpec {
}
class TestCase {
+doBefore()
+doAfter()
}
class Matchers << (T,trait) >> {
}

class BeforeAndAfter << (T,trait) >> {
}

class GivenWhenThen << (T,trait) >> {
}

class Matchers << (T,trait) >> {
}

class BeforeAndAfterAll << (T,trait) >> {
}
Matchers --o TestCase
BeforeAndAfter --o TestCase
GivenWhenThen --o TestCase
BeforeAndAfterAll --o TestCase
FeatureSpec <|-- TestCase
MocoServer <-- TestCase
TrapServer <-- TestCase
}
TestCase<|--MyTestCase
CustomerCare--o MyTestCase
GenericDevice--o MyTestCase
Esim--o MyTestCase
Oauth--o MyTestCase

Package DataBaseUtil <<Database>>{

object DataBase {
+getRecordsFromTable()
}



object SesDataInjection {
+generateData()
#generateGenericDevice()
#generateOem()
}
}

Package MiscUtil {
  object TelParser {
  +isActionExistInXml()
  }


  object FeatureSwitch {
    +getStringConfiguration()

  }

}

Package ServerMisc {
  object ScalaProxy{
  +getConfigValue()
  +getCounter()
  +getTsCurrentTimeMillis()
  +decryptCBCForEncIv()
  +getConfigValue()
  +sendScalaProxyRequest()
  }

  object Server{
  +writeFile()
  +executeCommand()
  }



  object SoapProxyConfig{
  +updateECASCertificate()
  }

  Server<--SoapProxyConfig
}

DataBaseUtil -- MyTestCase
MiscUtil  -- MyTestCase
ServerMisc-- MyTestCase
{% endplantuml %}
