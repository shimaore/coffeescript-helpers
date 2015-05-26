CoffeeScript helpers
====================

`require('coffeescript-helpers').helpers` contains the helpers installed by CoffeeScript.

    @helpers = helpers = """
    var slice = [].slice;
    var hasProp = {}.hasOwnProperty;
    var bind = function(fn, me){
    return function(){ return fn.apply(me, arguments); };
    };
    var extend = function(child, parent) {
    for (var key in parent) {
    if (hasProp.call(parent, key)) child[key] = parent[key];
    }
    function ctor() { this.constructor = child; }
    ctor.prototype = parent.prototype;
    child.prototype = new ctor();
    child.__super__ = parent.prototype;
    return child;
    };
    var indexOf = [].indexOf || function(item) {
    for (var i = 0, l = this.length; i < l; i++) {
    if (i in this && this[i] === item) return i;
    } return -1; };
    var modulo = function(a, b) { return (+a % (b = +b) + b) % b; };
    """.replace /\n/g, ''

`require('coffeescript-helpers').p_fun` will map a (compiled) CoffeeScript function to a JavaScript function with the proper helpers installed. It is intended to be used for CouchDB `map` functions, for example.
Usage: `p_fun(f)`, `p_fun(extra,f)`

    @p_fun = (extra,f) ->
      if not f?
        f = extra
        extra = ''
      return "(function (){#{extra} #{helpers} return #{f};})();"
