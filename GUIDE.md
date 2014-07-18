# Ad@m Android Publisher SDK Guide

  
사이트/앱 운영정책에 어긋나는 경우 적립금 지급이 거절 될 수 있으니 유의하시기 바랍니다.


이 문서는 Daum 신디케이션 제휴 당사자에 한해 제공되는 자료로 가이드 라인을 포함한 모든 자료의 지적재산권은 주식회사 다음커뮤니케이션이 보유합니다.


---

## Ad@m 광고 삽입 방법

### Ad@m SDK 구성

* Ad@mPublisherSDK.jar : Ad@m 광고를 삽입해주는 라이브러리

실제 광고를 다운로드 받고, 수익창출을 위해서 mobile.biz.daum.net 에서 사이트/앱 등록 후 client ID 를 발급받아야 한다. 아래 URL 을 통해 애플리케이션을 등록할 수 있다.

Ad@mPublisherSDK 를 프로젝트 내에 라이브러리로 Import 한다.


- 아래 세 가지 필수 권한을 AndroidManifist.xml 에 추가한다.
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
	```

	**SDK 2.3.0 부터는 최소한의 권한으로 INTERNET, ACCESS_NETWORK_STATE 권한을 설정해야 한다. 필수 권한 미 설정시 정상적 광고 노출 되지 않는다.**

- 광고를 넣을 Activity 에 반드시 android:configChanges=”orientation” 을 설정해준다.
			android:icon="@drawable/icon"
			android:label="@string/appName" >
				android:name=".TestAppActivity"
				android:configChanges="orientation|keyboardHidden"
				android:label="@string/appName" >
				<intent-filter>
	
				android:name="net.daum.adam.publisher.impl.AdInterstitialActivity"
				android:configChanges="orientation|keyboardHidden"
				android:screenOrientation="portrait" />
	
				android:configChanges="orientation|keyboardHidden" />


##### 4-a. Xml 방식
* Layout 의 main.xml 에서 광고가 노출되고자 하는 곳에 AdView 객체를 추가한다.  
* _광고를 노출 가능한 최소크기(320x50)보다 작게 광고 뷰가 할당되는 경우에는 광고가 노출되지 않을 수 있다._
* 그 이외의 속성 값은 어플리케이션의 특성에 따라 자유롭게 변경 가능하다.
**res/layout/main.xml**

<pre><code>&lt;RelativeLayout 
	android:layout_width="fill_parent"
	android:layout_height="fill_parent">

	&lt;net.daum.adam.publisher.AdView
		android:id="@+id/adview"
		android:visibility="invisible"
		android:layout_width="wrap_content" 
		android:layout_height="wrap_content" 
		android:layout_alignParentBottom="true"
		clientId=”TestClientId”
		requestInterval=”60”/>
&lt;/RelativeLayout></code></pre>




**[YourApplication]Activity.java**

	public class BannerTypeXML1 extends Activity {
	
	

			// 광고 리스너 설정
			// 1. 광고 클릭시 실행할 리스너
				@Override
			// 2. 광고 내려받기 실패했을 경우에 실행할 리스너
				@Override
			// 3. 광고를 정상적으로 내려받았을 경우에 실행할 리스너  
			adView.setOnAdLoadedListener(new OnAdLoadedListener() {
			});  

			adView.setOnAdWillLoadListener(new OnAdWillLoadListener() {

			adView.setOnAdClosedListener(new OnAdClosedListener() {



			adView.setAdCache(false);
			
			// Animation 효과 : 기본 값은 AnimationType.NONE

* AdView.OnAdLoadedListener : 광고가 내려받았을 경우 실행할 리스너

광고를 넣고자 하는 view 가 들어 있는 Activity 가 생성될 때 AdView 객체를 생성하고 광고 요청을 위해 광고 View 에 필요한 리스너와 할당 받은 ClientId 를 설정 한다. XML 레이아웃을 이용해 광고 생성할 때와 거의 동일하다.

<pre><code>
public class BannerTypeJava extends Activity {

**Interstitial(전면형) 광고는 당분간 Ad@m 의 네트워크 파트너를 대상으로 노출된다. Ad@m 의 네트워크 파트너가 아닐 경우에도 Expandable(확장형), Animated Banner (애니메이션형)형의 Rich Media 광고가 노출된다.**

Interstitial(전면형) 광고를 넣고자 하는 Activity 가 생성될 때 AdInterstitial 객체를 생성하고 광고 요청을 위해 필요한 리스너와 할당 받은 ClientId 를 설정한다. 이때, 반드시 3 단계에 명시한 XML 코드를 AndroidManifest.xml 에 반드시 추가해야 한다.

<pre><code>public class InterstitialActivity extends Activity {


Ad@m 은 유효 광고의 100% 노출을 보장하지 않습니다. 유효 광고 노출율은 송출 가능한 광고의 총 수량과 광고 호출수에 따라 달라지게 됩니다. 광고의 총 수량은 한정되어 있으나, 이에 비해 광고의 호출수가 많기 때문에 유효 광고의 수신에 실패하는 경우가 자주 발생할 수 있습니다. 또한 시간대나 앱의 종류, 날짜에 따라서도 노출 가능한 광고의 수가 달라질 수 있습니다.

Interstitial 광고의 경우에는 하우스 애드가 지원되지 않으므로, 일정 시간 이후 다시 호출해야 합니다.

내부적으로 인터넷 연결이 끊기면 광고 송출이 자동으로 중지되고, 연결되면 자동으로 광고 송출을 시작하게 됩니다.

### Q3. 광고 영역이 텅 비어보입니다. 아담 버그 아닌가요?

최초 광고를 내려받기 전 까지는 광고 요청에 시간이 걸리기 때문에 잠시 비어있을 수 있습니다. SDK 2.0 부터는 기본적으로 광고를 감싸고 있는 영역이 View.GONE 상태였다가, 광고가 완전히 내려받은 후에 View.VISIBLE 로 바꾸고 있습니다. 한번 View.VISIBLE 로 바뀐 영역은 광고 내려받기가 실패할 지라도 다시 가리지 않습니다.

### Q4. 시스템 앱에서 Expandable 광고가 보이지 않습니다. 왜 그런건가요?

키보드 앱과 같은 시스템 앱에서는 기존 광고 영역을 자유롭게 변경하기 어려운 관계로 Expandable(확장형) 광고를 보여주기 어렵습니다. 따라서 시스템 앱에서는 단순 클릭형 배너만 노출됩니다.

### Q5. Build PATH 에 라이브러리가 있는데도 앱 사용시 오류가 납니다.

Android Tools 버전 17 부터는 libs 폴더에 있는 라이브러리는 앱에 자동으로 포함됩니다.[^1]

만약 libs 폴더에 있지 않은 경우에는 해당 라이브러리가 Export 되고 있는지 반드시 확인해야 합니다.

[^1]: http://tools.android.com/recent/dealingwithdependenciesinandroidprojects