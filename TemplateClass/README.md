## JavaScript 페이지 꾸미기

- WrapBootstrap.com 
- Ruby - Asset Pipeline



1. Ruby Asset Pipeline

- 주요한기능

> 애셋 파이프라인의 첫번째 기능은 애셋사용





2. Scripts 추가

- Gemfile 추가

```
gem 'bootstrap' # bootstrap
gem 'font-awesome-rails' #fa
gem 'simple-line-icons-rails' #icon
```

app > assets > stylesheets > application.css 파일 확장명 application.scss로 변환

- 추가한 gem을 호출

```
@import 'bootstrap';
@import 'font-awesome';
@import 'simple-line-icons';
```

- javascripts > application.js > 수정

```
//= require jquery
//= require jquery_ujs
//= require popper  
//= require bootstrap
//= require turbolinks
//= require_tree .
```

- Vendor에 css파일 추가

  - 사용할 Template > html > vendor > assets > vendor폴더에서 각각 css파일 찾아서,  

    내 project폴더의 vendor > assets > stylesheets > assets 에 넣는다 (min파일은 넣지 않는다.)

  - windows 폴더간 이동시키는 것이 편함.

  - 첫 두 줄은 gem으로 호출하므로, 안넣어도 된다.

  - icon-hs , icon-line-pro 폴더 생성 후 안에 넣어준다. icon파일 중복되므로 폴더로 구분

```
    <link rel="stylesheet" href="../../assets/vendor/icon-awesome/css/font-awesome.min.css">
    <link rel="stylesheet" href="../../assets/vendor/icon-line/css/simple-line-icons.css">
    <link rel="stylesheet" href="../../assets/vendor/icon-line-pro/style.css">
    <link rel="stylesheet" href="../../assets/vendor/icon-hs/style.css">
    <link rel="stylesheet" href="../../assets/vendor/animate.css">
    <link rel="stylesheet" href="../../assets/vendor/hs-megamenu/src/hs.megamenu.css">
    <link rel="stylesheet" href="../../assets/vendor/hamburgers/hamburgers.min.css">
    <link rel="stylesheet" href="../../assets/vendor/chosen/chosen.css">
    <link rel="stylesheet" href="../../assets/vendor/slick-carousel/slick/slick.css">
    <link rel="stylesheet" href="../../assets/vendor/fancybox/jquery.fancybox.min.css">

    <!-- CSS Unify Theme -->
    <link rel="stylesheet" href="assets/css/styles.multipage-real-estate.css">

```



app > assets > stylesheets > applications.scss 에 추가

```
@import 'icon-line-pro/style';
@import 'icon-hs/style';
@import 'animate';
@import 'hs.megamenu';
@import 'hamburgers';
@import 'chosen';
@import 'slick';
@import 'jquery.fancybox';
@import 'styles.multipage-real-estate';
```

+++

- header 삽입

home-page-1. html > header전체 복사 => 

Project > shared폴더 생성 > _nav.html.erb 생성



- Category & Rent Products 삽입

home > index.html.erb 에 삽입

>home-page-1. html  
>
>507번째 줄(Category) ~ 1098번째 줄(Rent Products) 



- footer 삽입

shared > _footer.html.erb생성 

> home-page-1. html 
>
> footer 복사 (2051~2148줄)



- views > layouts > application.html.erb 수정 

```
<%= render 'shared/nav' %>
<%= yield %>
<%= render 'shared/footer' %>
```



3. jquery 추가

- gemfile 추가  해당 

```
# JS
gem 'jquery-migrate-rails'
```



- app > assets > javascripts > applitcation.js  에 

  //= require jquery-migrate 추가

```
//= require jquery
//= require jquery-migrate
//= require jquery_ujs
//= require popper
//= require bootstrap
//= require turbolinks
//= require_tree .
```

> 이 순서 반드시 지킬 것



- vendor > assets > javascripts 내에 옮기기

  - 폴더 째로 복사

    jquery-ui > ui > widgets

    js > components

    js > helpers

  - 파일복사

    js > hs.core

    js > widget

    jquery-ui > ui  core~widget파일 모두 복사해서 ui폴더 새로 생성 후 

    javascripts 내에 붙여넣기

    vendor > chosen.jquery

    vendor > hs.megamenu

    vendor > jquery.fancybox

    vendor > slick

- javascripts > applications.js  수정

```
//
//= require hs.megamenu
//= require widget
//= require widgets/menu
//= require widgets/mouse
//= require widgets/slider

//= require jquery
//= require slick
//= require jquery.fancybox
//= require hs.core
//= require components/hs.header
//= require helpers/hs.hamburgers
//= require components/hs.dropdown
//= require components/hs.slider
//= require components/hs.select
//= require components/hs.carousel
//= require components/hs.popup
//= require components/hs.go-to

//= require jquery
//= require jquery-migrate
//= require jquery_ujs
//= require popper
//= require bootstrap
//= require turbolinks
//= require_tree .
```

- layouts > application.html.erb 에 스크립트 호출코드 삽입

```
<%= yield :scripts %>
```

​		- render: 파일로 가져오는 형태.

​           	- yield: scripts 찾게한다.



- home > index.html.erb 최하단에 추가 (html의 JS Plugins init부분)

```
<% content_for :scripts do %>
    <!-- JS Plugins Init. -->
    <script>
      $(document).on('ready', function () {
        // initialization of header
        $.HSCore.components.HSHeader.init($('#js-header'));
        $.HSCore.helpers.HSHamburgers.init('.hamburger');

        // initialization of HSMegaMenu component
        $('.js-mega-menu').HSMegaMenu({
          event: 'hover',
          pageContainer: $('.container'),
          breakpoint: 991
        });

        // initialization of HSDropdown component
        $.HSCore.components.HSDropdown.init($('[data-dropdown-target]'), {
          afterOpen: function () {
            $(this).find('input[type="search"]').focus();
          }
        });

        // initialization of range slider
        $.HSCore.components.HSSlider.init('#rangeSlider1');

        // initialization of custom select
        $.HSCore.components.HSSelect.init('.js-custom-select');

        // initialization of carousel
        $.HSCore.components.HSCarousel.init('[class*="js-carousel"]');

        // initialization of popups
        $.HSCore.components.HSPopup.init('.js-fancybox');

        // initialization of go to
        $.HSCore.components.HSGoTo.init('.js-go-to');
      });
    </script>
<% end %>
```

- rake assets: precompile



- 코드정리 - index.html.erb (중복되는 article 제거 후 , 반복문적용)

  ```
  index페이지입니다.

  <!-- Category -->
  <div class="container g-py-100">
    <ul class="row no-gutters list-inline g-brd-y g-brd-gray-light-v3 g-px-0 mb-0">
      <!-- Category - List -->
      <li class="col-6 col-lg-3 list-inline-item u-block-hover g-brd-x g-brd-bottom g-brd-bottom-none--lg g-brd-gray-light-v3 g-mr-0">
        <a class="d-block g-bg-primary--hover g-text-underline--none--hover g-transition-0_3 g-px-50 g-pt-100 g-pb-50" href="#">
          <span class="d-inline-block g-color-text g-color-white-opacity-0_7--hover g-font-size-40 mb-2">
            <i class="icon-real-estate-077 u-line-icon-pro"></i>
          </span>
          <span class="d-block g-color-main g-color-white--hover g-font-weight-500 g-font-size-22">New Buildings</span>
          <span class="d-block g-color-text g-color-white--hover g-color-white-opacity-0_7--hover g-mb-70">in London</span>
          <span class="d-block g-color-main g-color-white--hover g-font-size-13">View More</span>
        </a>
      </li>
      <!-- End Category - List -->

      <!-- Category - List -->
      <li class="col-6 col-lg-3 list-inline-item u-block-hover g-brd-right g-brd-bottom g-brd-none--lg g-brd-gray-light-v3 g-mr-0">
        <a class="d-block g-bg-primary--hover g-text-underline--none--hover g-transition-0_3 g-px-50 g-pt-100 g-pb-50" href="#">
          <span class="d-inline-block g-color-text g-color-white-opacity-0_7--hover g-font-size-40 mb-2">
            <i class="icon-real-estate-050 u-line-icon-pro"></i>
          </span>
          <span class="d-block g-color-main g-color-white--hover g-font-weight-500 g-font-size-22">To New Comers</span>
          <span class="d-block g-color-text g-color-white--hover g-color-white-opacity-0_7--hover g-mb-70">in Neighborhood</span>
          <span class="d-block g-color-main g-color-white--hover g-font-size-13">View More</span>
        </a>
      </li>
      <!-- End Category - List -->

      <!-- Category - List -->
      <li class="col-6 col-lg-3 list-inline-item u-block-hover g-brd-x g-brd-gray-light-v3 g-mr-0">
        <a class="d-block g-bg-primary--hover g-text-underline--none--hover g-transition-0_3 g-px-50 g-pt-100 g-pb-50" href="#">
          <span class="d-inline-block g-color-text g-color-white-opacity-0_7--hover g-font-size-40 mb-2">
            <i class="icon-real-estate-070 u-line-icon-pro"></i>
          </span>
          <span class="d-block g-color-main g-color-white--hover g-font-weight-500 g-font-size-22">Cottages</span>
          <span class="d-block g-color-text g-color-white--hover g-color-white-opacity-0_7--hover g-mb-70">in Countryside</span>
          <span class="d-block g-color-main g-color-white--hover g-font-size-13">View More</span>
        </a>
      </li>
      <!-- End Category - List -->

      <!-- Category - List -->
      <li class="col-6 col-lg-3 list-inline-item u-block-hover g-brd-right g-brd-gray-light-v3 g-mr-0">
        <a class="d-block g-bg-primary--hover g-text-underline--none--hover g-transition-0_3 g-px-50 g-pt-100 g-pb-50" href="#">
          <span class="d-inline-block g-color-text g-color-white-opacity-0_7--hover g-font-size-40 mb-2">
            <i class="icon-real-estate-022 u-line-icon-pro"></i>
          </span>
          <span class="d-block g-color-main g-color-white--hover g-font-weight-500 g-font-size-22">To Owners</span>
          <span class="d-block g-color-text g-color-white--hover g-color-white-opacity-0_7--hover g-mb-70">of Properties</span>
          <span class="d-block g-color-main g-color-white--hover g-font-size-13">View More</span>
        </a>
      </li>
      <!-- End Category - List -->
    </ul>
  </div>
  <!-- End Category -->

  <!-- Rent Products -->
  <div class="container g-pb-100">
    <!-- Heading -->
    <div class="text-center g-mb-50">
      <span class="u-icon-v1 u-icon-size--xl g-color-primary"><i class="icon-real-estate-015 u-line-icon-pro"></i></span>
      <h2>Properties for Rent</h2>
    </div>
    <!-- End Heading -->

    <div class="row g-mb-40">

    <% 6.times do %>
      <div class="col-sm-6 col-lg-4 g-mb-30">
        <!-- Products -->
        <article>
          <!-- Products - Carousel Image -->
          <div class="js-carousel"
               data-infinite="true"
               data-arrows-classes="u-arrow-v1 g-pos-abs g-bottom-0 g-width-30 g-height-30 g-color-main g-bg-primary-opacity-0_4 g-bg-primary--hover g-color-white"
               data-arrow-left-classes="fa fa-angle-left g-right-30 g-brd-right g-brd-white-opacity-0_1"
               data-arrow-right-classes="fa fa-angle-right g-right-0">
            <div class="js-slide g-pos-rel">
              <a class="js-fancybox d-block u-icon-v3 g-width-30 g-height-30 g-color-main g-bg-primary-opacity-0_6 g-bg-primary--hover g-color-white g-font-size-14 g-pos-abs g-bottom-0 g-right-65" href="javascript:;"
                 data-fancybox="lightbox-gallery--02"
                 data-src="assets/img-temp/600x550/img1.jpg"
                 data-speed="350"
                 data-caption="Lightbox Gallery">
                <i class="icon-size-fullscreen"></i>
              </a>
              <img class="img-fluid" src="assets/img-temp/600x550/img1.jpg" alt="Image Description">
            </div>
            <div class="js-slide g-pos-rel">
              <a class="js-fancybox d-block u-icon-v3 g-width-30 g-height-30 g-color-main g-bg-primary-opacity-0_6 g-bg-primary--hover g-color-white g-font-size-14 g-pos-abs g-bottom-0 g-right-65" href="javascript:;"
                 data-fancybox="lightbox-gallery--02"
                 data-src="assets/img-temp/600x550/img2.jpg"
                 data-speed="350"
                 data-caption="Lightbox Gallery">
                <i class="icon-size-fullscreen"></i>
              </a>
              <img class="img-fluid" src="assets/img-temp/600x550/img2.jpg" alt="Image Description">
            </div>
            <div class="js-slide g-pos-rel">
              <a class="js-fancybox d-block u-icon-v3 g-width-30 g-height-30 g-color-main g-bg-primary-opacity-0_6 g-bg-primary--hover g-color-white g-font-size-14 g-pos-abs g-bottom-0 g-right-65" href="javascript:;"
                 data-fancybox="lightbox-gallery--02"
                 data-src="assets/img-temp/600x550/img3.jpg"
                 data-speed="350"
                 data-caption="Lightbox Gallery">
                <i class="icon-size-fullscreen"></i>
              </a>
              <img class="img-fluid" src="assets/img-temp/600x550/img3.jpg" alt="Image Description">
            </div>
            <div class="js-slide g-pos-rel">
              <a class="js-fancybox d-block u-icon-v3 g-width-30 g-height-30 g-color-main g-bg-primary-opacity-0_6 g-bg-primary--hover g-color-white g-font-size-14 g-pos-abs g-bottom-0 g-right-65" href="javascript:;"
                 data-fancybox="lightbox-gallery--02"
                 data-src="assets/img-temp/600x550/img4.jpg"
                 data-speed="350"
                 data-caption="Lightbox Gallery">
                <i class="icon-size-fullscreen"></i>
              </a>
              <img class="img-fluid" src="assets/img-temp/600x550/img4.jpg" alt="Image Description">
            </div>
          </div>
          <!-- Products - Carousel Image -->

          <div class="g-brd-x g-brd-gray-light-v3 g-bg-white">
            <!-- Products - List of Details -->
            <ul class="d-flex list-inline g-brd-y g-brd-gray-light-v3 mb-0">
              <li class="list-inline-item col-4 g-font-weight-500 g-font-size-13 text-center g-px-0 g-py-17 mr-0">
                <i class="align-middle g-color-text mr-1 icon-hotel-restaurant-022 u-line-icon-pro"></i>
                4 Bed rm.
              </li>
              <li class="list-inline-item col-4 g-font-weight-500 g-font-size-13 text-center g-px-0 g-brd-x g-brd-gray-light-v3 g-py-17 mr-0">
                <i class="align-middle g-color-text mr-1 icon-hotel-restaurant-008 u-line-icon-pro"></i>
                3 Bath rm.
              </li>
              <li class="list-inline-item col-4 g-font-weight-500 g-font-size-13 text-center g-px-0 g-py-17 mr-0">
                <i class="align-middle g-color-text mr-1 icon-real-estate-047 u-line-icon-pro"></i>
                2,020 sqft
              </li>
            </ul>
            <!-- End Products - List of Details -->

            <!-- Products - Address -->
            <div class="g-pa-25">
              <h3 class="g-font-weight-600 g-font-size-16">90 Karl Walk, Port Layla, BL2 0UR</h3>
              <p class="g-font-size-13 mb-4">London, England</p>
              <p class="g-color-text g-font-weight-500 g-font-size-13 mb-1">Agency: <a class="g-color-text g-color-primary--hover g-font-weight-400 g-text-underline--none--hover" href="#">Excellent Estate</a></p>
              <p class="g-color-text g-font-weight-500 g-font-size-13 mb-0">Posted: <span class="g-color-text g-font-weight-400">12 days ago</a></p>
            </div>
            <!-- End Products - Address -->

            <!-- Products - List of Details -->
            <ul class="d-flex list-inline align-items-center g-brd-top g-brd-gray-light-v3 mb-0">
              <li class="list-inline-item col-8 g-font-weight-600 g-font-size-17 text-center g-px-0 g-py-13 mr-0">
                $7,800<span class="g-font-size-13">/mo</span>
              </li>
              <li class="list-inline-item col-2 g-px-0 mr-0">
                <a class="d-block g-brd-x g-brd-gray-light-v3 g-color-text g-color-primary--hover g-font-weight-500 g-font-size-17 text-center g-text-underline--none--hover g-py-13" href="javascript:void(0);"
                   data-toggle="tooltip"
                   data-placement="top"
                   title="Add to Compare">
                  <i class="icon-finance-149 u-line-icon-pro"></i>
                </a>
              </li>
              <li class="list-inline-item col-2 g-px-0 mr-0">
                <a class="d-block g-color-text g-color-primary--hover g-font-weight-500 g-font-size-17 text-center g-text-underline--none--hover g-py-13" href="javascript:void(0);"
                   data-toggle="tooltip"
                   data-placement="top"
                   title="Save to Wishlist">
                  <i class="icon-medical-022 u-line-icon-pro"></i>
                </a>
              </li>
            </ul>
            <!-- End Products - List of Details -->
          </div>
          <a class="btn btn-block g-brd-top-none g-brd-gray-light-v3 g-brd-primary--hover g-color-white--hover g-bg-secondary g-bg-primary-dark-v1--hover g-font-weight-600 rounded-0 g-px-18 g-py-15" href="page-single-item-1.html">
            See Details
            <i class="align-middle ml-2 fa fa-angle-right"></i>
          </a>
        </article>
        <!-- End Products -->
      </div>
    <%end%>
    </div>

    <div class="text-center">
      <a class="btn g-color-text g-color-primary--hover g-bg-transparent g-font-weight-600 text-uppercase rounded-0" href="page-listing-1.html">
        <span class="align-middle g-color-primary g-font-size-20 g-pos-rel g-top-minus-2 mr-2">&#43;</span>
        See All Rent Listings
      </a>
    </div>
  </div>
  <!-- End Rent Products -->
  ```




  <% content_for :scripts do %>

  <!-- JS Plugins Init. -->
  <script>
    $(document).on('ready', function () {
      // initialization of header
      $.HSCore.components.HSHeader.init($('#js-header'));
      $.HSCore.helpers.HSHamburgers.init('.hamburger');
    
      // initialization of HSMegaMenu component
      $('.js-mega-menu').HSMegaMenu({
        event: 'hover',
        pageContainer: $('.container'),
        breakpoint: 991
      });
    
      // initialization of HSDropdown component
      $.HSCore.components.HSDropdown.init($('[data-dropdown-target]'), {
        afterOpen: function () {
          $(this).find('input[type="search"]').focus();
        }
      });
    
      // initialization of range slider
      $.HSCore.components.HSSlider.init('#rangeSlider1');
    
      // initialization of custom select
      $.HSCore.components.HSSelect.init('.js-custom-select');
    
      // initialization of carousel
      $.HSCore.components.HSCarousel.init('[class*="js-carousel"]');
    
      // initialization of popups
      $.HSCore.components.HSPopup.init('.js-fancybox');
    
      // initialization of go to
      $.HSCore.components.HSGoTo.init('.js-go-to');
    });
  </script>

  <% end %>

  ```

  

-  apllication.js  

  ```

//= require jquery
//= require jquery-migrate
//= require jquery_ujs
//= require popper
//= require bootstrap
//= require turbolinks
//= require_tree .

//= require hs.megamenu
//= require widget
//= require widgets/menu
//= require widgets/mouse
//= require widgets/slider

//= require jquery
//= require slick
//= require jquery.fancybox

//= require hs.core
//= require components/hs.header
//= require helpers/hs.hamburgers
//= require components/hs.dropdown
//= require components/hs.slider
//= require components/hs.select
//= require components/hs.carousel
//= require components/hs.popup
//= require components/hs.go-to
```

- 오류 수정 - img src 해결
  - multipage\real-estate\assets img-temp 복사
  - 내 Project 내 app > assets > images 내에 img-temp 붙여넣기

> app > assets > images 아래에 img-temp넣어주면된다. 
>
> asset_path의 default 위치가 app/assets/images

```
 data-src="<%=asset_path 'img-temp/600x550/img1.jpg' %>"

<img class="img-fluid" src="<%=asset_path 'img-temp/600x550/img1.jpg' %>" alt="Image Description"> 
```

- rake assets:clobber   =>  precompile된 assets을 비워준다. 

> 변경사항 반영하기 위해

- rake assets:precompile      =>  페이지 로딩속도 높여준다



스크립트 반영 순서 : css  >  js  >  font



4.font수정 - 오류수정

- html폴더 내에서 검색
  - Simple-Line-Icons 파일들(5개) 복사 =>  내 project > public > fonts폴더생성 후 안에 붙여넣기
  - finance 파일들(4개) 복사 => 내 project > public > fonts >finance 폴더 생성 후 안에 붙여넣기 (fonts 방금 생성된 폴더 내에 생성 )
  - hotel-restaurant 파일들(4개) 복사 => 내 project > public > fonts >hotel-restaurant 폴더 생성 후 안에 붙여넣기

> 같은 방식으로 fonts오류 모두 수정한다.

communication 파일들(4개) 복사 => 내 project > public > fonts >communication 폴더 생성 후 안에 붙여넣기

hs-icons 파일들(4개) 복사 => 내 project > public > fonts >hs-icons 폴더 생성 후 안에 붙여넣기

medical-and-health 파일들(4개) 복사 => 내 project > public > fonts >medical-and-health 폴더 생성 후 안에 붙여넣기

- 경로 수정
- 각 fonts url 수정

Vendor 내 오류발생하는 font 검색해서 url수정  => url을  font_url 로 수정

(ex hotel-restaurant, hs-icons ..)

```
font-family: "medical-and-health";
	src:font_url("medical/medical-and-health.eot");
	src:font_url("medical/medical-and-health.eot?#iefix") format("embedded-opentype"),
	font_url("medical/webfont/fonts/medical-and-health.woff") format("woff"),
	font_url("medical/webfont/fonts/medical-and-health.ttf") format("truetype"),
	font_url("medical/webfont/fonts/medical-and-health.svg#medical-and-health") format("svg");
	font-weight: normal;
```



0000000000000000



- search

  shared 아래 _search_form.html.erb 생성

  home/index.html.erb에 search

```

  ```

  routes 수정

  ```

  ```

  search.html.erb에  filter , list view, JS Plugins 복사

  ```
  <% content_for :scripts do %>

  ```

  ​

  chosen에러 수정

  - 내 project/vendor 에서 chosen-sprite 검색 후, 

  ```
  변경 전
  url('chosen-sprite.png')

  변경 후
  asset_url('chosen-sprite.png')
  ```

   >asset_url 로 명시해야 precompile시에도 어떤 파일인지 명시된다.

estate 폴더에서 chosen-sprite 검색해 이미지파일 2개 복사 후,

내 project/assets/images 안 에 붙여넣기



vender > jquery > ui  내 javascript파일 모두 복사 

내 project  / vendor > javascripts > ui폴더 생성 후 , 붙여넣기



page - list -view - 1. html 내 경로 복사 > 내 project > apps > javascripts >

application.js 내 붙여넣기 후 수정

수정 후 

  ```


//= require jquery
//= require jquery-migrate
//= require jquery_ujs
//= require popper
//= require bootstrap
//= require turbolinks
//= require_tree .

//= require hs.megamenu
//= ui/widget
//= ui/version
//= ui/keycode
//= ui/position
//= ui/unique-id
//= ui/safe-active-element
//= ui/widgets/menu
//= ui/widgets/mouse
//= ui/widgets/datepicker

//= require jquery
//= require slick
//= require jquery.fancybox

//= require hs.core
//= require components/hs.header
//= require helpers/hs.hamburgers
//= require components/hs.dropdown
//= require components/hs.slider
//= require components/hs.select
//= require components/hs.carousel
//= require components/hs.popup
//= require components/hs.datepicker
//= require components/hs.go-to
```





## Template Class 

- template license: 사용했던 라인센스가 구입한 템플릿의 라이센서와 일치하는지 
- Single / Multiple

1.vendor 파일에는 js, css library를 넣어두면 된다.

- 수정이 많이 발생하지 않을 파일들만 vendor폴더에

-  application.scss

- custom이 필요한 부분은 application.scss파일에서 

  혹은 custom.scss 파일 분리한 후에 

```
  @import 'custom'; 내용 추가
  ```

2.script파일은 반드시 js plugin init 이전에 load가 끝나야 함

app/views/layout/application.html.erb

  ```
<%= yield :scripts %> <!-- 본문 내용중 <%

<% content_for :scripts for do %>
<script>
//자바스크립트 실행문
</script>
<%end%>
```



3.css파일과 js파일 이름은 asset_pipeline에 소갛는 순간(vendor 폴더에 들어가는 순간) precomepile되어 이름이 알 수 없는 이름이 됨

- 절대 원래 이름으로 찾을 수 없음(이미지도 포함)
- asset_path, font_url, image_url 등의 view helper로 원래의 이름에서 바뀐 이름이 무엇인지 연결해줘야 함



4.vendor에 들어있는 파일의 숫자와 application.scss, application.js파일에 import/require 되는 파일의 숫자가 많아지면 반드시 서버시작 이전에 다음 과정을 수행한다.

```
rake assets:precompile
```

- 만약 프리컴파일 이후에 vendor 파일에 있는 내용물이 수정될 경우

```
rake assets:clobber #precompile된 내용 삭제
rake assets:precompile #다시 precompile
```

- 위 과정을 수행하지 않을 경우, 사양이 낮은 컴퓨터에서는 작업이 killed될 수 있다.




```