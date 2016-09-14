//---*******---
<set>lsbegin[=]-300</set>
<set>lssize[=]300</set>
<set>makelower[=]false</set>
<set>nocookiedelete[=]true</set>
<set>redirect[=]true</set>
<set>litelog[=]true</set>
<loadcookie>!</loadcookie>

<headers>
  Host: *******
  User-Agent: Mozilla/5.0 (Windows NT 6.1; rv:45.0) Gecko/20100101 Firefox/45.0
  Accept: */*
  Accept-Language: ru-RU,ru;q=0.8,en-US;q=0.5,en;q=0.3
  Accept-Encoding: gzip, deflate
  Content-Type: application/x-www-form-urlencoded; charset=UTF-8
  X-Requested-With: XMLHttpRequest
  Connection: keep-alive
</headers>

//=================================
//  *******-= Авторизация =-*******
//=================================	

 <set>get[=]http://[UserSite]/</set>
 <process>target[=]result</process>
 <check>target[=]Onlinx</check>
 <notchecked>
 <parse>echo[=]-= Проблема с сайтом ! =-</parse>
 </notchecked>
 <checked>
    <check>target[=]href="/logout</check>
    <checked>cookielogin[=]true</checked> 
        <ifnotcookielogin>
        <set>post[=]http://[UserSite]/ajax</set>
        <set>data[=]login=[UserName]&password=[UserPass]&type=auth</set>
        <process>target[=]result</process>
        <set>get[=]http://[UserSite]/account</set>
        <process>target[=]result</process>
        <check>target[=][UserName]</check>
            <checked>
            <savecookie>!</savecookie>
            </checked>
        </ifnotcookielogin>

  <set>get[=]http://[UserSite]/account</set>
  <process>target[=]result</process>
  
	
  <parse>wmr[=]target|Баланс для вывода:|class="fa fa-rouble|<td>| <i|</parse>
    <parse>echo[=]======================</parse>
    <parse>echo[=]Здравствуйте [UserName]!</parse>
    <parse>echo[=]======================</parse>

//================================
//  *******-=    Серф    =-*******
//================================ 	  
label1:
  <set>get[=]http://[UserSite]/account/earn</set>
  <process>target[=]result</process>
  <check>target[=]account/view</check>
  <checked>
      <parse>echo[=] -= Серфим. =-</parse>
      <parse>ad[=]target|href="/account/view/|" target</parse>
      <set>get[=]http://[UserSite]/account/view/[ad]</set>
      <process>target[=]result</process>
          <check>target[=]выход</check>
          <notchecked>
              <parse>timer[=]target|app.timer(|);</parse>
                  <set>waittime[=][timer]</set>
                  <set>waitmult[=]1000</set>
                  <wait>!</wait> 
              <set>post[=]http://[UserSite]/ajax</set>
              <set>data[=]type=user&user=get_cap</set>
              <process>target[=]result</process>
              <parse>captcha[=]target|<img id="captcha" src="/captcha.php?|"></parse>
              <set>get[=]http://[UserSite]/captcha.php?[captcha]</set>
              <setcaptcha>mask=444;filtermode=10;</setcaptcha>
              <process>cap[=]captcha</process>
              <set>post[=]http://[UserSite]/ajax</set>
              <set>data[=]captcha=[cap]&type=user&user=view</set>
              <process>target[=]result</process>
          </notchecked>
          <checked>
              <parse>captcha[=]target|<img id="captcha" src="/captcha.php?|"></parse>
              <set>get[=]http://[UserSite]/captcha.php?[captcha]</set>
              <setcaptcha>mask=444;filtermode=10;</setcaptcha>
              <process>cap[=]captcha</process>
              <set>post[=]http://[UserSite]/ajax</set>
              <set>data[=]captcha=[cap]&type=user&user=view</set>
              <process>target[=]result</process>
          </checked>
          <check>target[=]status":"success"</check>
          <checked>
              <parse>echo[=]Просмотр зачислен.</parse>
          </checked> 	
          <goto>label1</goto>               
  </checked> 
  <notchecked> 
                <parse>echo[=] -=   Нет серфа.   =-</parse>
  </notchecked>       
                <parse>echo[=] -= Ждём 5 минут. =-</parse>       
                <set>waittime[=]300</set>
                <set>waitmult[=]1000</set>
                <wait>!</wait>
</checked> 
