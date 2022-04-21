<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>안동버스</title>
<script src="https://releases.jquery.com/git/jquery-git.min.js"></script>
<link rel="manifest" href="manifest.json" />
<script>
$(function() {
  if(window.location.hash!="") {
	var snap = $(window.location.hash).offset().left;
	$('snap-tabs').scrollLeft(snap);
  }
  $('style').append("@media(prefers-reduced-motion:no-preference){.scroll-snap-x{scroll-behavior:smooth;}");
  $('#stop .box').on('propertychange change keyup paste input', function() {
	var value=$(this).val();
	if(value.length > 1)
	$.ajax({
	  type:"GET",
	  url:"//andong.city/callback.php",
	  data:"rq=search&what=stop&str="+value,
	  success:function(res) {
		var out="";
		for (var i in res) {
		  out += "<li class=\"content\">";
		  out += "<a href=\"/stop/?Number=" + res[i].Number + "\">";

		  out += "<div class=\"w\">";
		  out += "<div class=\"label\">" + res[i].Label + "</div>";
		  out += "</div>";
		  out += "<div class=\"num\">" + res[i].Mobile + "</div>";

		  out += "</a></li>";
		}
		$('#stop .search').html(out);
	  }
	});
  });
  $('#bus .box').on('propertychange change keyup paste input', function() {
	var value=$(this).val();
	if(value.length > 0) $.ajax({
	type:"GET",
	url:"http://andong.city/callback.php",
	data:"rq=search&what=bus&str="+value,
	success:function(res) {
		var out="";
		for (var i in res) {
			out += "<li class=\"content\">";
			out += "<a href=\"/stop/?Number=" + res[i].Number + "\">";

			switch(res[i].Number.slice(0,1)) {
				case '급':
				routeTypeNum="FAST";
				break;
				case '순':
				routeTypeNum="ROUND";
				break;
				case '임':
				routeTypeNum="6";
				break;
				case '길':
				routeTypeNum="6";
				break;
				case '예':
				routeTypeNum="5";
				break;
				case '북':
				routeTypeNum="3";
				break;
				default:
				routeTypeNum=res[i].Number.slice(0,1);
			}
		out += "<div class=\"w\">";
		out += "<span class=\"label busType_"+routeTypeNum+"\">" + res[i].Number + "</span>";
		out += "<span class=\"se\">";
		out += res[i].Start + " - " + res[i].End;
		out += "</span>";
		out += "</div>";

		out += "</a></li>";
		}
		$('#bus .search').html(out);
	}});
});
	if(navigator.geolocation) {
		navigator.geolocation.watchPosition(function(pos) {
			var Lat=pos.coords.latitude;
			var Lon=pos.coords.longitude;
			console.log(Lat);
			console.log(Lon);

			$.ajax({
			type:"GET",
			url:"//andong.city/callback.php",
			data:"rq=list&lat="+Lat+"&lon="+Lon,
			success:function(obj) {
				var out="";
				for (var i in obj) {
					out += "<li class=\"content\"><a href=\"/stop/?Number=" + obj[i].Number + "\">";

					out += "<div class=\"w\">";
					out += "<div class=\"label\">" + obj[i].Label + "</div>";
					out += "<div class=\"dist\">";
					var pD =Math.round(obj[i].Distance/10)*10-10;
					if(pD < 10) out += "현위치";
					else out += pD + "m 이내";
					out += "</div>";

					out += "</div>";
					out += "<div class=\"num\">" + obj[i].Mobile + "</div>";

					out += "</a></li>";
				}
				$('.list').html(out);
				}
			});
		});
	}
});
</script>
<style>
@import url("//fonts.googleapis.com/css2?family=Noto+Sans+KR&display=swap");
:root {
  --body-background-color: #ddd;
  --header-background-color: #603;
  --gray-color: #999;
  --white-color: #fff;
}
body {
  font-family: "Noto Sans KR", sans-serif;
  background-color: var(--body-background-color);
  margin: 0;
  overscroll-behavior-y: none;
}
snap-tabs {
  display: flex;
  flex-direction: column;
  overflow: hidden;
  position: relative;
}
snap-tabs > section {
	block-size: 100%;
}
snap_tabs > header {
	flex-shrink: 0;
	min-block-size: fit-content;
}
header {
  background-color: var(--header-background-color);
  width: 100vmin;
  display: flex;
  flex-direction: column;
}
header svg {
  width: 23vmin;
  margin: 0.5em 0 0 0.8em;
  color:var(--white-color);
}
nav {
  display:flex;
  flex-direction:row;
}
nav a {
  flex:1;
  text-align:center;
  text-decoration:none;
  font-size:0.8em;
  padding:1.5em 0;
  color:var(--white-color);
}
section {
  block-size: 100%;
  display: grid;
  grid-auto-flow: column;
  grid-auto-columns: 100%;
}
#stop .list,
#stop .search,
#bus .search {
  padding: 0;
  margin: .7em 0;
  width: 100vmin;
  display: flex;
  flex-direction: column;
}
#stop .list .content,
#stop .search .content,
#bus .search .content {
  list-style: none;
  margin: 0;
  border-bottom: 1px solid var(--body-background-color);
  align-items: center;
}
#stop .list .content a,
#stop .search .content a,
#bus .search .content a {
  display: block;
  text-decoration: none;
  padding: 0.7em 1em;
  background-color: var(--white-color);
  color: #000;
}
#stop .list .content .w,
#stop .search .content .w,
#bus .search .content .w {
  display: flex;
  align-content: center;
}
#bus .search .content .w {
  padding: 0.2em 0;
}
#stop .list .content .w .label,
#stop .search .content .w .label {
  font-size: 0.8em;
  display: inline-flex;
}
#bus .search .content .w .label {
  padding-right: 0.6em;
  font-size: 0.9em;
  display: inline-flex;
}
#bus .search .content .w .se {
  padding: 0.1em 1em;
  font-size: 0.7em;
  display: inline-flex;
  color: var(--gray-color);
}
#stop .list .content .w .dist {
  display: inline-flex;
  margin-left:1em;
  padding: 0.2em 0.3em 0;
  text-align: center;
  border: 1px solid var(--gray-color);
  color: var(--gray-color);
  font-size: 0.55em;
}
#stop .list .content .num,
#stop .search .content .num {
  color: var(--gray-color);
  font-size: 0.7em;
  margin-top: 0.2em;
}
#stop .box,
#bus .box {
  width:91vmin;
  margin:0.3em 3vmin;
  border:0;
  font-size:0.8em;
  padding:.8em 1.5vmin;
}
#bus .box {
  margin-top: 0.9em;
}
.scroll-snap-x {
  overflow: hidden;
  overscroll-behavior-x: contain;
  scroll-snap-type: x mandatory;
  @media (hover: none) {
	scrollbar-width: none;

	&::-webkit-scrollbar {
	  width: 0;
	  height: 0;
	}
  }
}

.busType_0,
.busType_1 {color:#9E1030}
.busType_2 {color:#EA5632}
.busType_3 {color:#FFAC25}
.busType_4 {color:#9C9A40}
.busType_5 {color:#4F84C4}
.busType_6 {color:#223A52}
.busType_9 {color:#672E3B}
.busType_FAST {color:#BF2E1B}
.busType_ROUND {color:#757}
</style>
</head>
<body>
<header>
 <svg version="1.0" viewBox="0 0 1200 800" xmlns="http://www.w3.org/2000/svg">
<g transform="translate(0 800) scale(.1 -.1)" xmlns="http://www.w3.org/2000/svg">
<circle cx="1100" cy="5400" r="700" fill="none" stroke="#fff" stroke-width="500"/>
<circle cx="4730" cy="1800" r="700" fill="none" stroke="#fff" stroke-width="500"/>
<path d="m2450 5110v-1330h560v1640h3280l400 250v720l-460-450h-3210v650h-560z" fill="#fff" />
<path d="m10110 5490c0-113-4-180-10-180-5 0-10-18-10-40s-4-40-10-40c-5 0-10-18-10-40s-4-40-10-40c-5 0-10-9-10-20s-4-20-10-20c-5 0-10-9-10-20s-4-20-10-20c-5 0-10-9-10-20s-4-20-10-20c-5 0-10-9-10-20s-4-20-10-20c-5 0-10-6-10-14 0-7-13-27-30-44-16-18-30-37-30-42 0-15-209-220-224-220-6 0-18-9-26-20s-21-20-30-20-22-9-30-20-21-20-30-20-22-9-30-20-22-20-32-20-18-4-18-10c0-5-9-10-20-10s-20-4-20-10c0-5-9-10-20-10s-20-4-20-10c0-5-13-10-30-10-16 0-30-4-30-10 0-5-9-10-20-10s-20-4-20-10c0-5-13-10-30-10-16 0-30-4-30-10 0-5-13-10-30-10-16 0-16-3-30-10 0-5-9-10-40-17v680h-560v-690h-660v690h-480v-690h-660v690h-480v-1730h-1340v440h1040v420h-2040v460h2040v420h-2580v-1300h1040v-440h-3300v800h-560v-1200h7280v1020h700v-1880h560v860h2780v400h-2780v640h30c17 0 30 5 30 10 0 6 18 10 40 10s40 5 40 10c0 6 18 10 40 10s40 5 40 10c0 6 14 10 30 10 17 0 30 5 30 10 0 6 14 10 30 10 17 0 30 5 30 10 0 6 14 10 30 10 17 0 30 5 30 10 0 6 9 10 20 10s20 5 20 10c0 6 6 10 14 10 16 0 140 62 144 73 2 4 12 7 23 7 10 0 19 5 19 10 0 6 9 10 20 10s20 4 20 8c0 5 12 15 28 23 15 8 34 22 43 31 9 10 22 18 29 18 6 0 25 14 40 30 15 17 33 30 39 30 13 0 221 206 221 218 0 5 14 23 30 40 17 18 30 36 30 40 0 5 14 23 30 40 17 18 30 37 30 43 0 11 24 39 34 39 3 0 6-8 6-18s9-24 20-32 20-21 20-30 9-22 20-30 20-21 20-28 18-31 40-52 40-43 40-49c0-13 105-121 118-121 4 0 31-22 60-50 29-27 58-50 65-50s19-9 27-20 21-20 30-20 22-9 30-20 22-20 32-20 18-4 18-10c0-5 6-10 13-10s23-9 35-20 28-20 37-20c8 0 15-4 15-10 0-5 9-10 20-10s20-4 20-10c0-5 9-10 20-10s20-4 20-10c0-5 9-10 20-10s20-4 20-10c0-5 14-10 30-10 17 0 30-4 30-10 0-5 9-10 20-10s20-4 20-10c0-5 15-10 34-10s38-4 41-10c3-5 19-10 36-10 16 0 29-4 29-10 0-5 14-10 30-10 17 0 30-4 30-10 0-5 18-10 40-10s40-4 40-10c0-5 17-10 38-10 40 0 82 33 82 65 0 8 5 15 10 15 6 0 10 9 10 20s5 20 10 20c6 0 10 9 10 20s5 20 10 20c6 0 10 9 10 20s5 20 10 20c6 0 10 9 10 20s5 20 10 20c6 0 10 6 10 13s9 23 20 35 20 28 20 37c0 8 5 15 10 15 6 0 10 14 10 30 0 29-1 30-50 30-27 0-50 5-50 10 0 6-18 10-40 10s-40 5-40 10c0 6-18 10-40 10s-40 5-40 10c0 6-13 10-30 10-16 0-30 5-30 10 0 6-9 10-20 10s-20 5-20 10c0 6-13 10-30 10-16 0-30 5-30 10 0 6-9 10-20 10s-20 5-20 10c0 6-9 10-20 10s-20 5-20 10c0 6-9 10-20 10s-20 5-20 10c0 6-9 10-20 10s-20 5-20 10c0 6-9 10-20 10s-20 5-20 10c0 6-8 10-18 10s-24 9-32 20-21 20-28 20-31 18-52 40-43 40-48 40c-14 0-142 126-142 140 0 7-13 25-30 40-16 15-30 34-30 42 0 7-9 20-20 28s-20 22-20 32-4 18-10 18c-5 0-10 9-10 20s-4 20-10 20c-5 0-10 9-10 20s-4 20-10 20c-5 0-10 9-10 20s-4 20-10 20c-5 0-10 14-10 30 0 17-4 30-10 30-5 0-10 18-10 40s-4 40-10 40-10 73-10 200v200h-540v-180zm-2660-1840v-290h-660v620h660z" fill="#fff" />
</g>
</svg>
  <nav>
	  <a href="#home">홈</a>
	  <a href="#stop">정류장</a>
	  <a href="#bus">버스</a>
	  <a href="#more">더보기</a>
	</nav>
	<span class="snap-indicator"></span>
</header>
<snap-tabs class="scroll-snap-x">
<section>
  <article id="home">
  </article>
  <article id="stop">
	<ul class="list"></ul>
	<input class="box" placeholder="정류장 이름 또는 번호를 검색합니다" type="text">
	<ul class="search"></ul>
  </article>
  <article id="bus">
	<input class="box" placeholder="버스 노선 번호를 검색합니다" type="text">
	<ul class="search"></ul>
  </article>
  <article id="more">
  </article>
</section>
</snap-tabs>
</body>
</html>
