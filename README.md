# tips

javascript:(function () %7B%0A%09if (window.subvaAllowRightClick === undefined) %7B%0A%09%09// https://greasyfork.org/en/scripts/23772-absolute-enable-right-click-copy/code%0A%09%09window.subvaAllowRightClick = function (dom) %7B%0A%09%09%09(function GetSelection() %7B%0A%09%09%09%09var Style = dom.createElement('style');%0A%09%09%09%09Style.type = 'text/css';%0A%09%09%09%09var TextNode = '*%7Buser-select:text!important;-webkit-user-select:text!important;%7D';%0A%09%09%09%09if (Style.styleSheet) %7B%0A%09%09%09%09%09Style.styleSheet.cssText = TextNode;%0A%09%09%09%09%7D%0A%09%09%09%09else %7B%0A%09%09%09%09%09Style.appendChild(dom.createTextNode(TextNode));%0A%09%09%09%09%7D%0A%09%09%09%09dom.getElementsByTagName('head')%5B0%5D.appendChild(Style);%0A%09%09%09%7D)();%0A%0A%09%09%09(function SetEvents() %7B%0A%09%09%09%09var events = %5B'copy', 'cut', 'paste', 'select', 'selectstart'%5D;%0A%09%09%09%09for (var i = 0; i < events.length; i++)%0A%09%09%09%09%09dom.addEventListener(events%5Bi%5D, function (e) %7B%0A%09%09%09%09%09%09e.stopPropagation();%0A%09%09%09%09%09%7D, true);%0A%09%09%09%7D)();%0A%0A%09%09%09(function RestoreEvents() %7B%0A%09%09%09%09var n = null;%0A%09%09%09%09var d = document;%0A%09%09%09%09var b = dom.body;%0A%09%09%09%09var SetEvents = %5Bd.oncontextmenu = n, d.onselectstart = n, d.ondragstart = n, d.onmousedown = n%5D;%0A%09%09%09%09var GetEvents = %5Bb.oncontextmenu = n, b.onselectstart = n, b.ondragstart = n, b.onmousedown = n, b.oncut = n, b.oncopy = n, b.onpaste = n%5D;%0A%09%09%09%7D)();%0A%0A%09%09%09(function RightClickButton() %7B%0A%09%09%09%09setTimeout(function () %7B%0A%09%09%09%09%09dom.oncontextmenu = null;%0A%09%09%09%09%7D, 2000);%0A%09%09%09%09function EventsCall(callback) %7B%0A%09%09%09%09%09this.events = %5B'DOMAttrModified', 'DOMNodeInserted', 'DOMNodeRemoved', 'DOMCharacterDataModified', 'DOMSubtreeModified'%5D;%0A%09%09%09%09%09this.bind();%0A%09%09%09%09%7D%0A%0A%09%09%09%09EventsCall.prototype.bind = function () %7B%0A%09%09%09%09%09this.events.forEach(function (event) %7B%0A%09%09%09%09%09%09dom.addEventListener(event, this, true);%0A%09%09%09%09%09%7D.bind(this));%0A%09%09%09%09%7D;%0A%09%09%09%09EventsCall.prototype.handleEvent = function () %7B%0A%09%09%09%09%09this.isCalled = true;%0A%09%09%09%09%7D;%0A%09%09%09%09EventsCall.prototype.unbind = function () %7B%0A%09%09%09%09%09this.events.forEach(function (event) %7B%0A%09%09%09%09%09%7D.bind(this));%0A%09%09%09%09%7D;%0A%09%09%09%09function EventHandler(event) %7B%0A%09%09%09%09%09this.event = event;%0A%09%09%09%09%09this.contextmenuEvent = this.createEvent(this.event.type);%0A%09%09%09%09%7D%0A%0A%09%09%09%09EventHandler.prototype.createEvent = function (type) %7B%0A%09%09%09%09%09var target = this.event.target;%0A%09%09%09%09%09var event = target.ownerDocument.createEvent('MouseEvents');%0A%09%09%09%09%09event.initMouseEvent(type, this.event.bubbles, this.event.cancelable,%0A%09%09%09%09%09%09target.ownerDocument.defaultView, this.event.detail,%0A%09%09%09%09%09%09this.event.screenX, this.event.screenY, this.event.clientX, this.event.clientY,%0A%09%09%09%09%09%09this.event.ctrlKey, this.event.altKey, this.event.shiftKey, this.event.metaKey,%0A%09%09%09%09%09%09this.event.button, this.event.relatedTarget);%0A%09%09%09%09%09return event;%0A%09%09%09%09%7D;%0A%09%09%09%09EventHandler.prototype.fire = function () %7B%0A%09%09%09%09%09var target = this.event.target;%0A%09%09%09%09%09var contextmenuHandler = function (event) %7B%0A%09%09%09%09%09%09event.preventDefault();%0A%09%09%09%09%09%7D.bind(this);%0A%09%09%09%09%09target.dispatchEvent(this.contextmenuEvent);%0A%09%09%09%09%09this.isCanceled = this.contextmenuEvent.defaultPrevented;%0A%09%09%09%09%7D;%0A%09%09%09%09window.addEventListener('contextmenu', handleEvent, true);%0A%09%09%09%09function handleEvent(event) %7B%0A%09%09%09%09%09event.stopPropagation();%0A%09%09%09%09%09event.stopImmediatePropagation();%0A%09%09%09%09%09var handler = new EventHandler(event);%0A%09%09%09%09%09window.removeEventListener(event.type, handleEvent, true);%0A%09%09%09%09%09var EventsCallBback = new EventsCall(function () %7B%0A%09%09%09%09%09%7D);%0A%09%09%09%09%09handler.fire();%0A%09%09%09%09%09window.addEventListener(event.type, handleEvent, true);%0A%09%09%09%09%09if (handler.isCanceled && (EventsCallBback.isCalled))%0A%09%09%09%09%09%09event.preventDefault();%0A%09%09%09%09%7D%0A%09%09%09%7D)();%0A%0A%09%09%09// function KeyPress(e) %7B%0A%09%09%09// %09if (e.altKey && e.ctrlKey) %7B%0A%09%09%09// %09%09if (confirm("Activate Absolute Right Click Mode!") === true) %7B%0A%09%09%09// %09%09%09Absolute_Mod();%0A%09%09%09// %09%09%7D%0A%09%09%09// %09%7D%0A%09%09%09// %7D%0A%09%09%09// dom.addEventListener("keydown", KeyPress);%0A%0A%09%09%09(function Absolute_Mod() %7B%0A%09%09%09%09var events = %5B'contextmenu', 'copy', 'cut', 'paste', 'mouseup', 'mousedown', 'keyup', 'keydown', 'drag', 'dragstart', 'select', 'selectstart'%5D;%0A%09%09%09%09for (var i = 0; i < events.length; i++) %7B%0A%09%09%09%09%09dom.addEventListener(events%5Bi%5D, function (e) %7B%0A%09%09%09%09%09%09e.stopPropagation();%0A%09%09%09%09%09%7D, true);%0A%09%09%09%09%7D%0A%09%09%09%7D)();%0A%09%09%7D;%0A%0A//%09%09window.subvaAllowRightClick(document);%0A%0A%09%09function runAll(w) %7B%0A%09%09%09try %7B%0A%09%09%09%09window.subvaAllowRightClick(w.document);%0A%09%09%09%7D catch (e) %7B%0A%09%09%09%7D%0A%09%09%09for (var i = 0; i < w.frames.length; i++) %7B%0A%09%09%09%09runAll(w.frames%5Bi%5D);%0A%09%09%09%7D%0A%09%09%7D%0A%09%7D%0A%09runAll(window);%0A%7D)();
