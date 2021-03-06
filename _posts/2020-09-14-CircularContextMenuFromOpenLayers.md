---
title: OpenLayers 4 - 마커위에 원형 메뉴 띄우기
updated: 2020-09-14 16:47
category: OpenLayers
---
**참고문서** : [OpenLayers 4.6.5 API - Example : Popup](https://www.giserdqy.com/wp-content/guids/ol-v4.6.5/examples/popup.html?q=popup)   
   
### OpenLayers(v4.6.5) - 마커 클릭 시 원형 메뉴 띄우기 
   
- 이전에 구현했던 [원형 컨텍스트 메뉴](https://rlawjddbs.github.io/CircularMenu)를 응용하여 `OpenLayers` 라이브러리에 적용
- `GeoServer`에 추가한 `WMS Layer`를 OpenLayers의 로 등록하여 배경지도 구성
- `CORS`[^1] 정책 때문에 엄청 헤맴
	- 클라이언트의 요청 및 응답을 송수신하는 서버에 `proxy.jsp`를 구성하여 해당 `proxy.jsp`가 프록시로서 클라이언트의 요청을 받고 GeoServer의 데이터를 가져와 응답하도록 하여 CORS 정책 위반에 관한 에러를 방지 
- OpenLayers의 `Overlay`를 활용
   
#### 원형 컨텍스트 메뉴 구현 결과
![circularContextMenu](https://raw.githubusercontent.com/rlawjddbs/rlawjddbs.github.io/master/_posts/imgs/200914/circular_contextmenu.gif)   
> 1. cctv 버튼(`marker`) 클릭 시 원형 메뉴가 나타나야 함
> 2. 원형 메뉴는 메뉴를 닫기 전까지 지도내에서 화면을 스크롤하거나 확대 및 축소하여도 cctv 버튼에 `같은 크기로 항상 고정`되어 있어야 함
> 3. 원형 메뉴는 메뉴의 영역 밖을 클릭하기 전까지는 `close` 되지 않아야 함
> 4. 운영자가 원형 메뉴의 개수 및 각도를 `조절` 할 수 있어야 함 (커스터 마이징 기능 함수로 제공해야 함)
   
#### index.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">

<link rel="stylesheet" href="/ol.popup/lib/ol/css/ol.css" />
<link rel="stylesheet" href="/ol.popup/common/css/style.css" />

<title>OL popup Test Drive~!</title>
<script src="https://code.jquery.com/jquery-3.5.1.js" integrity="sha256-QWo7LDvxbWT2tbbQ97B53yJnYU3WhH/C8ycbRAkjPDc=" crossorigin="anonymous"></script>
<script src="/ol.popup/lib/ol/js/ol.js"></script>
<script src="/ol.popup/common/js/olprj.js"></script>
<script src="/ol.popup/common/js/OLUtil.js"></script>
<script src="/ol.popup/common/js/CircularContextMenu.js"></script>
<script src="/ol.popup/common/js/Main.js"></script>
</head>
<body>
    <div id="map" class="map"></div>
    <div id="popup" class="circular-menu" style="">
        <p class="localName">지역명</p>
        <ul>
        </ul>        
    </div>
</body>
<script type="text/javascript">
    olprj.cmmn.Main();
</script>
</html>
```
1. 지도를 시각화할 요소 `<div id="map" class="map">`
2. 원형 컨텍스트 메뉴를 구성할 요소 `<div id="popup" class="circular-menu">`
   
#### style.css
```css
@charset "UTF-8";
/* map */
.map {width: 100%; height: 400px; border: 1px solid #ccc; box-sizing:border-box;}
/* contextmenu */
.circular-menu{position:absolute; width: 70px; height: 70px; z-index:50; transform:translate3d(-50%, -50%, 0);}
.circular-menu ul{width: 100%; height:100%; margin:0; padding:0; border:2px solid #ffa500; border-radius:50px; box-sizing:border-box; background-color:rgba(255, 255, 255, .23);}
.circular-menu li{width:20px; height:20px; position:absolute; border-radius:10px; background:#fff; box-sizing:border-box; border:2px solid #ff0000; list-style:none; left:25px; top:25px; cursor:pointer;}
.circular-menu .localName{position: absolute; padding: 10px 20px; left: 50%; top: -45px; background: #fff; transform: translate3d(-50%, -50%, 0); box-shadow: 0 0 5px rgba(0, 0, 0, .22); border-radius: 0.3em;}

/* ===================================================================================================== */
/* popup 요소의 콘텐츠를 select(선택) 하려면 .ol-selectable 클래스의 CSS를 Override 한다. 기본값 (auto;) */
/* ===================================================================================================== */
/* .ol-selectable { user-select: text; } */
```
- 오픈레이어스 4.6.5 API 예제 중 popup 예제에 나온 스타일을 사용 (`.map`)
   
#### olprj.js
```javascript
(function(){
	"use strict";
	var _class = {
		version : "1.0" 
	}
	window.olprj = $.extend(window.olprj || {});
})();
```
   
#### OLUtil.js
```javascript
(function(){
	"use strict";
	
	var _class = function(){
		
		let _self = this;
		let _markerCnt = 0;
		
		// --------------------------------
		// Outer Function
		// --------------------------------
		this.initOverlay = function(option){
			return initOverlay(option);
		} // outer function initOverlay
		
		// ---------------------------------------------------------------------------------
		// Create Marker Point - LonLat : 경도(longitude)와 위도(latitude)를 합쳐 일컫는 말
		// ---------------------------------------------------------------------------------
		this.createMarker = function(longitude, latitude){
			return createMarker(longitude, latitude);
		}
		
		// --------------------------------
		// Inner Function
		// --------------------------------
		function initOverlay(option) {
			return new ol.Overlay({
				element : option.container,
				autoPan : true, // 오버레이 팝업 시 자동 맵 이동 활성화
				autoPanAnimation : { // 맵 이동 지연 시간 (밀리초)
					duration : 250
				}, // autoPanAnimation
				autoPanMargin : 60 // autoPan-margin 속성 (기본값 20, 수치가 클 수록 팝업 컨테이너 외부로 많이 이동함)
			}); // Overlay Constructor
		} // initOverlay
		
		// --------------------------------
		// Marker Creator
		// --------------------------------
		function createMarker(longitude, latitude) {
			return new ol.layer.Vector({
				id : "marker" + (_markerCnt++),
				title : "marker",
				source : new ol.source.Vector({
					features: [
						new ol.Feature({
							geometry: new ol.geom.Point([longitude, latitude]) // ol.geom.Point(coordinate 값) - ol.prj.fromLonLat() 함수는 왜 쓰는지? 확인 필요
						}) // Freature Constructor
					] // features arrays
				}) // Vector constructor
			}); // Vector constructor
		} // createMarker
		
		
	} // _class
	
	olprj.util = $.extend(olprj.util || {}, {
		OLUtil : _class
	});
	
})();
```
   
#### CircularContextMenu.js
```javascript
(function(){
    "use strict";

    const _class = function() {
        
        function initialize() {

            // -------------------------------
            // [1] draw & set the contextmenu
            // -------------------------------
            drawMenuComp(6);
            setMenuComp({ startAngle: 315, divCnt: 8 });

        }

        function drawMenuComp(liCnt) {

            var $compContainer = $(".circular-menu > ul");
            var html = "";
            for(var i = 0; i < liCnt; i++) {
                html += "<li></li>";
            }
            var $html = $(html);
            $compContainer.append($html);

        }

        /**
		 * 원형 메뉴 컴포넌트 설정
         * @param startAngle 시작 각도
         * @param divCnt 나눌 개수
         */
        function setMenuComp(option) {
            const circle = 360; // 360° degree
            var $comp = $(".circular-menu > ul").find("li"); // Circular Menu Component
            
            if (!option.startAngle) {
                option.startAngle = circle;
            } // end if

            if (option.startAngle > circle) {
                option.startAngle -= circle;
            } // end if
            
            if (!option.divCnt || option.divCnt < $comp.length) {
                option.divCnt = $comp.length;
            } // end if
            
            var division = circle / option.divCnt; // 각 메뉴의 각도 연산
            for(var i = 0; i < $comp.length; i++){
                option.startAngle += division;
                if(option.startAngle > circle) { option.startAngle %= circle; }
                $comp.eq(i).css({
                    "-webkit-transform": "rotate(" + option.startAngle + "deg) translate(2.2em) rotate("+ (-option.startAngle) + "deg)",
                    "-moz-transform": "rotate(" + option.startAngle + "deg) translate(2.2em) rotate("+ (-option.startAngle) + "deg)",
                    "-ms-transform": "rotate(" + option.startAngle + "deg) translate(2.2em) rotate("+ (-option.startAngle) + "deg)",
                    "-o-transform": "rotate(" + option.startAngle + "deg) translate(2.2em) rotate("+ (-option.startAngle) + "deg)",
                    "transform": "rotate(" + option.startAngle + "deg) translate(2.2em) rotate("+ (-option.startAngle) + "deg)",
                }); // css
            } // end for
        } // setMenuComp

        return initialize();

    };

    olprj.util = $.extend(olprj.util || {}, {
        CircularContextMenu : _class
    });
    
})();
```

#### Main.js
```javascript
(function(){
	"use strict";
	var _class = function() {
		
		// -----------------
		// OpenLayers Util
		// -----------------
		olprj.util.OLUtil = new olprj.util.OLUtil();
		olprj.util.CircularContextMenu();
		olprj.cmmn.Main = this;
		
		let _self = this; // Main Class
		let map = null; // Map Object
		
		let bounds = [224651.386, 423458.585, 268876.421, 464775.252]; // 확인 필요. 위도랑 경도값 외에 다른 값이 왜 필요한지?
		
		let marker = null; // The Markers to include in the overlay
		let container = null; // Popup Container
		let overlay = null; // Layer containing Popup element
		
		// -----------------
		// Outer Function
		// -----------------
		this.getMap = function() {
			return getMap();
		}
		
		// -----------------
		// Inner Function
		// -----------------
		function initialize() {
			createPopup(); 
			createOverlay(); // Overlay 생성
			createMap(); // Map 생성
			
			// ----------------
			// create marker
			// ----------------
			_self.marker = olprj.util.OLUtil.createMarker(243122.1394324466, 449707.71883182955);
			_self.map.addLayer(_self.marker);
			
			// Add temp Marker Layer
			_self.map.addLayer(olprj.util.OLUtil.createMarker(232719.25381420954, 451420.3776560798));
			_self.map.addLayer(olprj.util.OLUtil.createMarker(254830.63503240456, 448732.8061152156));
			_self.map.addLayer(olprj.util.OLUtil.createMarker(236933.85352493316, 440120.3608575003));
			
			initEvent();
		}
		
		function initEvent() {
			_self.map.on("click", function(evt){
				if (_self.map.hasFeatureAtPixel(evt.pixel) === true) {
					_self.map.forEachLayerAtPixel(evt.pixel, function(layer){
						if(layer.get("title") === "marker") {
							_self.content.innerHTML = layer.get("id"); // 지역명 표시
							var lonLat = layer.getSource().getFeatures()[0].getGeometry().getCoordinates(); // 클릭한 지점의 마커 좌표
							_self.overlay.setPosition(lonLat); // 팝업 메뉴가 담겨있는 오버레이 레이어 위치 변경
						} // end if
					});
				} else {
					_self.overlay.setPosition(undefined);
				}
			});
		} // initEvent
		
		function createMap() {
			_self.map = new ol.Map({
				layers: [ // Option 1
					new ol.layer.Image({
						source: new ol.source.ImageWMS({
							ratio: 1,
							// proxy.jsp를 통해 geoserver로 요청을 날림. 이 과정을 거치지 않으면 
							// CORS(Cross Origin Resource Sharing) 정책에 위반되어 예외가 발생함
							url: 'http://???.???.???.???:????/ol.popup/proxy.jsp?url=http://???.???.???.???:????/geoserver/TEST/wms',
							params: {
								'FORMAT': "image/png",
								'VERSION': '1.1.1',  
								"LAYERS": 'TEST:LARD_ADM_SECT_SGG',
								"exceptions": 'application/vnd.ogc.se_inimage'
							} // params
						}) // ImageWms Constructor
					}), // Image Constructor
					new ol.layer.Tile({
						visible: false,
						source: new ol.source.TileWMS({
							url: 'http://???.???.???.???:????/ol.popup/proxy.jsp?url=http://???.???.???.???:????/geoserver/TEST/wms',
							params: {
								"FORMAT" : "image/png",
								"VERSION" : "1.1.1",
								tiled: true,
								"LAYERS": "TEST:LARD_ADM_SECT_SGG",
								"exceptions": "application/vnd.ogc.se_inimage",
								titlesOrigin: 224651.386 + "," +423458.585,
								crossOrigin:"anonymous"
							} // params
						}) // TileJSON Constructor
					}) // Tile Constructor
				], // layers arrays
				overlays: [_self.overlay], // overlays props
				target: "map", // target props
				view: new ol.View({ 
					projection: new ol.proj.Projection({
						code: "EPSG:5186",
						units: "m",
						axisOrientation: "neu",
						global: false
					}),
					center: [0, 0],
					maxZoom:28,
					zoom: 12
				}) // view props
			}); // Map Constructor
			
			// fit the view
			_self.map.getView().fit(bounds, _self.map.getSize());
		} // createMap
		function createPopup() {
			_self.container = document.getElementById("popup");
		} // createMap
		function createOverlay() {
			_self.overlay = olprj.util.OLUtil.initOverlay({ container : _self.container });
		} // createOverlay
		function getMap() {
			return _self.map;
		} // getMap
		return initialize();
	} // _class
	olprj.cmmn = $.extend(olprj.cmmn || {}, {
		Main : _class
	}); // extend
})();
```
   
#### 수정해야 할 사항
1. 오픈레이어스의 `popup`은 지도를 알아서 해당 팝업쪽으로 스크롤하는데, `popup container` 영역 밖에 위치한 `지역명 말풍선`의 크기는 감안하지 않고 스크롤 된다. 지역명 말풍선까지 모두 노출되게 스크롤 할 수 있도록 수정해야 함   
	- `Overlay` 객체 생성 시 여러 옵션을 객체 형식의 인자로 입력할 수 있는데 이 때 옵션 키 `autoPanMargin` 을 이용하면 `popup container`의 실제 크기보다 외부적으로 더 큰 크기로 스크롤 할 수 있음을 확인하여 반영하였음. (2020-08)

[^1]: `Cross Origin Resource Sharing`. 직역하면 교차 출처 리소스 공유. 서비스를 제공하는 서버가 아닌 다른 서버를 통해 실제 서비스를 제공하는 서버로부터 데이터를 요청할 경우 브라우저는 CROS 정책을 위반한 것으로 간주하여 스크립트에서 시작한 `Http 요청`을 제한해 버린다.