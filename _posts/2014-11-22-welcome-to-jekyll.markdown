---
layout: post
title:  "ui-rect-select.js!"
date:   2014-11-22 13:40:29
categories: jekyll update
---

see example: [http://anypossiblew.github.io/rect-multi-select/](http://anypossiblew.github.io/rect-multi-select/)

## MainCtrl
{% highlight javascript %}
/**
 * @ngdoc function
 * @name rectMultiSelectApp.controller:MainCtrl
 * @description
 * # MainCtrl
 * Controller of the rectMultiSelectApp
 */
angular.module('rectMultiSelectApp')
  .controller('MainCtrl', function ($scope) {

    $scope.moveSelect = false;

    $scope.baseUrl = "http://static.photoshows.cn/";
    $scope.baseExt = "@1e_200w_200h_1c_1o.jpg";
    $scope.photos = [
      {name: '408.jpg'},{name: '409.jpg'},{name: '410.jpg'},{name: '411.jpg'},{name: '412.jpg'}
    ];

    $scope.selectedPhotos = [];

    $scope.setSelectable = function(selectable) {
      $scope.selectable = selectable;
    };
  });
{% endhighlight %}

## PhotoCtrl
{% highlight javascript %}
/**
 * @ngdoc function
 * @name rectMultiSelectApp.controller:PhotoctrlCtrl
 * @description
 * # PhotoctrlCtrl
 * Controller of the rectMultiSelectApp
 */
angular.module('rectMultiSelectApp')
  .controller('PhotoCtrl', ['$scope', '$log', function ($scope, $log) {

    $scope.select = function () {

      var photo = $scope.photo;
      //photo.actionSelected = !photo.actionSelected;
      if (photo.actionSelected) {
        $scope.setSelectable(true);
        $scope.selectedPhotos.push(photo);
      } else {
        for (var i = $scope.selectedPhotos.length - 1; i >= 0; i -= 1) {
          if($scope.selectedPhotos[i].name == photo.name) {
            delete $scope.selectedPhotos.splice(i, 1);
          }
        }
        if ($scope.selectedPhotos.length == 0) {
          $scope.setSelectable(false);
        }
      }
    };

    $scope.$watch('photo.actionSelected', function(selected) {
      $scope.select();
    });

  }]);
{% endhighlight %}

## Main.html
{% highlight html %}
<button class="btn "
        ng-click="moveSelect=!moveSelect"
        ng-class="{'btn-default': !moveSelect, 'btn-primary': moveSelect}">
  {{moveSelect?'实时选择 move select':'非实时选择 final select'}}
</button>
{% endhighlight %}

{% highlight html %}
<div class="jumbotron selectable-container"
     rect-multi-select="{'selector': '.photo'}"
     rect-move-select="{{moveSelect}}"
     rect-selector="photo.actionSelected">

  <div class="container">

    <div class="photo-wall photo-selectable-container">
      <div ng-repeat="photo in photos"
           ng-controller="PhotoCtrl"
           class="photo"
           ng-class="{'selectable': selectable, 'selected': photo.actionSelected}">
        <a href="">
          <img ng-src="{{baseUrl}}{{photo.name}}{{baseExt}}">
        </a>

        <div class="action action-select"
             ng-click="photo.actionSelected=!photo.actionSelected">
            <span class="icon-photo-select"
                  ng-class="{'icon-photo-selected': photo.actionSelected}"></span>
        </div>
      </div>
    </div>
  </div>

</div>
{% endhighlight %}

## css
### photo-selectable.scss
{% highlight scss %}
.photo-wall {
  .photo {
    position: relative;
    display: inline-block;
    margin: 1px;
    background-color: grey;
    width: 200px;
    height: 200px;
    img {
      width: 100%;
      height: 100%;
    }

  }
}

// photo selectable container
.photo-selectable-container {
  width: 100%;
  .photo {
    position: relative;
    border: 0;
    img {
      @include transition(all 218ms ease-in-out);
      @include box-sizing(border-box);
      border: 0;
    }

    .action-select {
      position: absolute;
      top: 5px;
      left: 5px;
      z-index: 2;
      @include opacity(0);
    }

    .action-container {
      @include transition(all 218ms ease-in-out);
    }

    &:hover, &.hover {
      .action {
        @include opacity(1);
      }
    }

    &.selectable {
      img {
        border: 5px solid $body-bg-color;
      }
      .action-select {
        @include opacity(1);
      }
      .action-container {
        border: 5px solid transparent;
      }
    }

    &.selected {
      img {
        border: 5px solid $theme-edit-color;
      }
    }
  }

  .icon-photo-select {
    @include opacity(.55);
    background: no-repeat url(../images/photos-action.png) -42px -50px;
    background-clip: content-box;
    cursor: pointer;
    position: absolute;
    left: 0;
    top: 0;
    width: 24px;
    height: 24px;
  }
  .icon-photo-select.icon-photo-selected {
    @include animation(ttPhotosSelectionOverlayCheckmarkSelectedTransition .3s linear 0s 1);
    @include opacity(1);
    background: no-repeat url(../images/photos-action.png) -109px -50px;
    background-clip: content-box;
  }
}
{% endhighlight %}

### rect-select.scss
{% highlight scss %}
.selectable-container {
  position: relative;
}

*[rect-multi-select] {
  .ghost-select {
    position: absolute;
    z-index: 9000;
    cursor: default !important;
    &.show {
      @include opacity(1);
    }
  }

  #big-ghost{
    background-color:rgba(239, 28, 190, 0.6);
    border:1px solid #aaf81a;
    position:absolute;
    @include opacity(0);
    &.show {
      @include opacity(1);
    }
  }

  .ghost-select > div {
    position: absolute;
    left: 0px !important;
    top: 2px !important;
  }

  .ghost-select > span {
    width: 100%;
    height: 100%;
    float: left;
    @include box-shadow(black 0 0 10px);
    @include opacity(.4);
    background-color: #8bf;
  }

  #grid {
    width: 100%;
    height: 100%;
    top: 0;
    position: absolute;
  }
}
{% endhighlight %}

Check out the [ui-rect-select docs][ui-rect-select] for more info on how to get the most out of ui-rect-select.

[ui-rect-select]:      http://anypossiblew.github.io/rect-multi-select
