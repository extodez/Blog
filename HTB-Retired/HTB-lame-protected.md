<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="robots" content="noindex, nofollow">
    <title>Password Protected Page</title>
    <style>
        html, body {
            margin: 0;
            width: 100%;
            height: 100%;
            font-family: Arial, Helvetica, sans-serif;
        }
        #dialogText {
            padding: 10px 30px;
            color: white;
            background-color: #333333;
        }
        
        #dialogWrap {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: table;
            background-color: #EEEEEE;
        }
        
        #dialogWrapCell {
            display: table-cell;
            text-align: center;
            vertical-align: middle;
        }
        
        #mainDialog {
            max-width: 400px;
            margin: 5px;
            border: solid #AAAAAA 1px;
            border-radius: 10px;
            box-shadow: 3px 3px 5px 3px #AAAAAA;
            margin-left: auto;
            margin-right: auto;
            background-color: #FFFFFF;
            overflow: hidden;
            text-align: left;
        }
        #passArea {
            padding: 20px 30px;
            background-color: white;
        }
        #passArea > * {
            margin: 5px auto;
        }
        #pass {
            width: 100%;
            height: 40px;
            font-size: 30px;
        }
        
        #messageWrapper {
            float: left;
            vertical-align: middle;
            line-height: 30px;
        }
        
        .notifyText {
            display: none;
        }
        
        #invalidPass {
            color: red;
        }
        
        #success {
            color: green;
        }
        
        #submitPass {
            font-size: 20px;
            border-radius: 5px;
            background-color: #E7E7E7;
            border: solid gray 1px;
            float: right;
        }
        #contentFrame {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
        #attribution {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            text-align: center;
            padding: 10px;
            font-weight: bold;
            font-size: 0.8em;
        }
        #attribution, #attribution a {
            color: #999;
        }
    </style>
  </head>
  <body>
    <iframe id="contentFrame" frameBorder="0" allowfullscreen></iframe>
    <div id="dialogWrap">
        <div id="dialogWrapCell">
            <div id="mainDialog">
                <div id="dialogText">This page is password protected.</div>
                <div id="passArea">
                    <p id="passwordPrompt">Password</p>
                    <input id="pass" type="password" name="pass" autofocus>
                    <div>
                        <span id="messageWrapper">
                            <span id="invalidPass" class="notifyText">Sorry, please try again.</span>
                            <span id="success" class="notifyText">Success!</span>
                            &nbsp;
                        </span>
                        <button id="submitPass" type="button">Submit</button>
                        <div style="clear: both;"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div id="attribution">
        Protected by <a href="https://www.maxlaumeister.com/pagecrypt/">PageCrypt</a>
    </div>
    <script>
/*
CryptoJS v3.1.2
code.google.com/p/crypto-js
(c) 2009-2013 by Jeff Mott. All rights reserved.
code.google.com/p/crypto-js/wiki/License
*/
var CryptoJS=CryptoJS||function(u,p){var d={},l=d.lib={},s=function(){},t=l.Base={extend:function(a){s.prototype=this;var c=new s;a&&c.mixIn(a);c.hasOwnProperty("init")||(c.init=function(){c.$super.init.apply(this,arguments)});c.init.prototype=c;c.$super=this;return c},create:function(){var a=this.extend();a.init.apply(a,arguments);return a},init:function(){},mixIn:function(a){for(var c in a)a.hasOwnProperty(c)&&(this[c]=a[c]);a.hasOwnProperty("toString")&&(this.toString=a.toString)},clone:function(){return this.init.prototype.extend(this)}},
r=l.WordArray=t.extend({init:function(a,c){a=this.words=a||[];this.sigBytes=c!=p?c:4*a.length},toString:function(a){return(a||v).stringify(this)},concat:function(a){var c=this.words,e=a.words,j=this.sigBytes;a=a.sigBytes;this.clamp();if(j%4)for(var k=0;k<a;k++)c[j+k>>>2]|=(e[k>>>2]>>>24-8*(k%4)&255)<<24-8*((j+k)%4);else if(65535<e.length)for(k=0;k<a;k+=4)c[j+k>>>2]=e[k>>>2];else c.push.apply(c,e);this.sigBytes+=a;return this},clamp:function(){var a=this.words,c=this.sigBytes;a[c>>>2]&=4294967295<<
32-8*(c%4);a.length=u.ceil(c/4)},clone:function(){var a=t.clone.call(this);a.words=this.words.slice(0);return a},random:function(a){for(var c=[],e=0;e<a;e+=4)c.push(4294967296*u.random()|0);return new r.init(c,a)}}),w=d.enc={},v=w.Hex={stringify:function(a){var c=a.words;a=a.sigBytes;for(var e=[],j=0;j<a;j++){var k=c[j>>>2]>>>24-8*(j%4)&255;e.push((k>>>4).toString(16));e.push((k&15).toString(16))}return e.join("")},parse:function(a){for(var c=a.length,e=[],j=0;j<c;j+=2)e[j>>>3]|=parseInt(a.substr(j,
2),16)<<24-4*(j%8);return new r.init(e,c/2)}},b=w.Latin1={stringify:function(a){var c=a.words;a=a.sigBytes;for(var e=[],j=0;j<a;j++)e.push(String.fromCharCode(c[j>>>2]>>>24-8*(j%4)&255));return e.join("")},parse:function(a){for(var c=a.length,e=[],j=0;j<c;j++)e[j>>>2]|=(a.charCodeAt(j)&255)<<24-8*(j%4);return new r.init(e,c)}},x=w.Utf8={stringify:function(a){try{return decodeURIComponent(escape(b.stringify(a)))}catch(c){throw Error("Malformed UTF-8 data");}},parse:function(a){return b.parse(unescape(encodeURIComponent(a)))}},
q=l.BufferedBlockAlgorithm=t.extend({reset:function(){this._data=new r.init;this._nDataBytes=0},_append:function(a){"string"==typeof a&&(a=x.parse(a));this._data.concat(a);this._nDataBytes+=a.sigBytes},_process:function(a){var c=this._data,e=c.words,j=c.sigBytes,k=this.blockSize,b=j/(4*k),b=a?u.ceil(b):u.max((b|0)-this._minBufferSize,0);a=b*k;j=u.min(4*a,j);if(a){for(var q=0;q<a;q+=k)this._doProcessBlock(e,q);q=e.splice(0,a);c.sigBytes-=j}return new r.init(q,j)},clone:function(){var a=t.clone.call(this);
a._data=this._data.clone();return a},_minBufferSize:0});l.Hasher=q.extend({cfg:t.extend(),init:function(a){this.cfg=this.cfg.extend(a);this.reset()},reset:function(){q.reset.call(this);this._doReset()},update:function(a){this._append(a);this._process();return this},finalize:function(a){a&&this._append(a);return this._doFinalize()},blockSize:16,_createHelper:function(a){return function(b,e){return(new a.init(e)).finalize(b)}},_createHmacHelper:function(a){return function(b,e){return(new n.HMAC.init(a,
e)).finalize(b)}}});var n=d.algo={};return d}(Math);
(function(){var u=CryptoJS,p=u.lib.WordArray;u.enc.Base64={stringify:function(d){var l=d.words,p=d.sigBytes,t=this._map;d.clamp();d=[];for(var r=0;r<p;r+=3)for(var w=(l[r>>>2]>>>24-8*(r%4)&255)<<16|(l[r+1>>>2]>>>24-8*((r+1)%4)&255)<<8|l[r+2>>>2]>>>24-8*((r+2)%4)&255,v=0;4>v&&r+0.75*v<p;v++)d.push(t.charAt(w>>>6*(3-v)&63));if(l=t.charAt(64))for(;d.length%4;)d.push(l);return d.join("")},parse:function(d){var l=d.length,s=this._map,t=s.charAt(64);t&&(t=d.indexOf(t),-1!=t&&(l=t));for(var t=[],r=0,w=0;w<
l;w++)if(w%4){var v=s.indexOf(d.charAt(w-1))<<2*(w%4),b=s.indexOf(d.charAt(w))>>>6-2*(w%4);t[r>>>2]|=(v|b)<<24-8*(r%4);r++}return p.create(t,r)},_map:"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/="}})();
(function(u){function p(b,n,a,c,e,j,k){b=b+(n&a|~n&c)+e+k;return(b<<j|b>>>32-j)+n}function d(b,n,a,c,e,j,k){b=b+(n&c|a&~c)+e+k;return(b<<j|b>>>32-j)+n}function l(b,n,a,c,e,j,k){b=b+(n^a^c)+e+k;return(b<<j|b>>>32-j)+n}function s(b,n,a,c,e,j,k){b=b+(a^(n|~c))+e+k;return(b<<j|b>>>32-j)+n}for(var t=CryptoJS,r=t.lib,w=r.WordArray,v=r.Hasher,r=t.algo,b=[],x=0;64>x;x++)b[x]=4294967296*u.abs(u.sin(x+1))|0;r=r.MD5=v.extend({_doReset:function(){this._hash=new w.init([1732584193,4023233417,2562383102,271733878])},
_doProcessBlock:function(q,n){for(var a=0;16>a;a++){var c=n+a,e=q[c];q[c]=(e<<8|e>>>24)&16711935|(e<<24|e>>>8)&4278255360}var a=this._hash.words,c=q[n+0],e=q[n+1],j=q[n+2],k=q[n+3],z=q[n+4],r=q[n+5],t=q[n+6],w=q[n+7],v=q[n+8],A=q[n+9],B=q[n+10],C=q[n+11],u=q[n+12],D=q[n+13],E=q[n+14],x=q[n+15],f=a[0],m=a[1],g=a[2],h=a[3],f=p(f,m,g,h,c,7,b[0]),h=p(h,f,m,g,e,12,b[1]),g=p(g,h,f,m,j,17,b[2]),m=p(m,g,h,f,k,22,b[3]),f=p(f,m,g,h,z,7,b[4]),h=p(h,f,m,g,r,12,b[5]),g=p(g,h,f,m,t,17,b[6]),m=p(m,g,h,f,w,22,b[7]),
f=p(f,m,g,h,v,7,b[8]),h=p(h,f,m,g,A,12,b[9]),g=p(g,h,f,m,B,17,b[10]),m=p(m,g,h,f,C,22,b[11]),f=p(f,m,g,h,u,7,b[12]),h=p(h,f,m,g,D,12,b[13]),g=p(g,h,f,m,E,17,b[14]),m=p(m,g,h,f,x,22,b[15]),f=d(f,m,g,h,e,5,b[16]),h=d(h,f,m,g,t,9,b[17]),g=d(g,h,f,m,C,14,b[18]),m=d(m,g,h,f,c,20,b[19]),f=d(f,m,g,h,r,5,b[20]),h=d(h,f,m,g,B,9,b[21]),g=d(g,h,f,m,x,14,b[22]),m=d(m,g,h,f,z,20,b[23]),f=d(f,m,g,h,A,5,b[24]),h=d(h,f,m,g,E,9,b[25]),g=d(g,h,f,m,k,14,b[26]),m=d(m,g,h,f,v,20,b[27]),f=d(f,m,g,h,D,5,b[28]),h=d(h,f,
m,g,j,9,b[29]),g=d(g,h,f,m,w,14,b[30]),m=d(m,g,h,f,u,20,b[31]),f=l(f,m,g,h,r,4,b[32]),h=l(h,f,m,g,v,11,b[33]),g=l(g,h,f,m,C,16,b[34]),m=l(m,g,h,f,E,23,b[35]),f=l(f,m,g,h,e,4,b[36]),h=l(h,f,m,g,z,11,b[37]),g=l(g,h,f,m,w,16,b[38]),m=l(m,g,h,f,B,23,b[39]),f=l(f,m,g,h,D,4,b[40]),h=l(h,f,m,g,c,11,b[41]),g=l(g,h,f,m,k,16,b[42]),m=l(m,g,h,f,t,23,b[43]),f=l(f,m,g,h,A,4,b[44]),h=l(h,f,m,g,u,11,b[45]),g=l(g,h,f,m,x,16,b[46]),m=l(m,g,h,f,j,23,b[47]),f=s(f,m,g,h,c,6,b[48]),h=s(h,f,m,g,w,10,b[49]),g=s(g,h,f,m,
E,15,b[50]),m=s(m,g,h,f,r,21,b[51]),f=s(f,m,g,h,u,6,b[52]),h=s(h,f,m,g,k,10,b[53]),g=s(g,h,f,m,B,15,b[54]),m=s(m,g,h,f,e,21,b[55]),f=s(f,m,g,h,v,6,b[56]),h=s(h,f,m,g,x,10,b[57]),g=s(g,h,f,m,t,15,b[58]),m=s(m,g,h,f,D,21,b[59]),f=s(f,m,g,h,z,6,b[60]),h=s(h,f,m,g,C,10,b[61]),g=s(g,h,f,m,j,15,b[62]),m=s(m,g,h,f,A,21,b[63]);a[0]=a[0]+f|0;a[1]=a[1]+m|0;a[2]=a[2]+g|0;a[3]=a[3]+h|0},_doFinalize:function(){var b=this._data,n=b.words,a=8*this._nDataBytes,c=8*b.sigBytes;n[c>>>5]|=128<<24-c%32;var e=u.floor(a/
4294967296);n[(c+64>>>9<<4)+15]=(e<<8|e>>>24)&16711935|(e<<24|e>>>8)&4278255360;n[(c+64>>>9<<4)+14]=(a<<8|a>>>24)&16711935|(a<<24|a>>>8)&4278255360;b.sigBytes=4*(n.length+1);this._process();b=this._hash;n=b.words;for(a=0;4>a;a++)c=n[a],n[a]=(c<<8|c>>>24)&16711935|(c<<24|c>>>8)&4278255360;return b},clone:function(){var b=v.clone.call(this);b._hash=this._hash.clone();return b}});t.MD5=v._createHelper(r);t.HmacMD5=v._createHmacHelper(r)})(Math);
(function(){var u=CryptoJS,p=u.lib,d=p.Base,l=p.WordArray,p=u.algo,s=p.EvpKDF=d.extend({cfg:d.extend({keySize:4,hasher:p.MD5,iterations:1}),init:function(d){this.cfg=this.cfg.extend(d)},compute:function(d,r){for(var p=this.cfg,s=p.hasher.create(),b=l.create(),u=b.words,q=p.keySize,p=p.iterations;u.length<q;){n&&s.update(n);var n=s.update(d).finalize(r);s.reset();for(var a=1;a<p;a++)n=s.finalize(n),s.reset();b.concat(n)}b.sigBytes=4*q;return b}});u.EvpKDF=function(d,l,p){return s.create(p).compute(d,
l)}})();
CryptoJS.lib.Cipher||function(u){var p=CryptoJS,d=p.lib,l=d.Base,s=d.WordArray,t=d.BufferedBlockAlgorithm,r=p.enc.Base64,w=p.algo.EvpKDF,v=d.Cipher=t.extend({cfg:l.extend(),createEncryptor:function(e,a){return this.create(this._ENC_XFORM_MODE,e,a)},createDecryptor:function(e,a){return this.create(this._DEC_XFORM_MODE,e,a)},init:function(e,a,b){this.cfg=this.cfg.extend(b);this._xformMode=e;this._key=a;this.reset()},reset:function(){t.reset.call(this);this._doReset()},process:function(e){this._append(e);return this._process()},
finalize:function(e){e&&this._append(e);return this._doFinalize()},keySize:4,ivSize:4,_ENC_XFORM_MODE:1,_DEC_XFORM_MODE:2,_createHelper:function(e){return{encrypt:function(b,k,d){return("string"==typeof k?c:a).encrypt(e,b,k,d)},decrypt:function(b,k,d){return("string"==typeof k?c:a).decrypt(e,b,k,d)}}}});d.StreamCipher=v.extend({_doFinalize:function(){return this._process(!0)},blockSize:1});var b=p.mode={},x=function(e,a,b){var c=this._iv;c?this._iv=u:c=this._prevBlock;for(var d=0;d<b;d++)e[a+d]^=
c[d]},q=(d.BlockCipherMode=l.extend({createEncryptor:function(e,a){return this.Encryptor.create(e,a)},createDecryptor:function(e,a){return this.Decryptor.create(e,a)},init:function(e,a){this._cipher=e;this._iv=a}})).extend();q.Encryptor=q.extend({processBlock:function(e,a){var b=this._cipher,c=b.blockSize;x.call(this,e,a,c);b.encryptBlock(e,a);this._prevBlock=e.slice(a,a+c)}});q.Decryptor=q.extend({processBlock:function(e,a){var b=this._cipher,c=b.blockSize,d=e.slice(a,a+c);b.decryptBlock(e,a);x.call(this,
e,a,c);this._prevBlock=d}});b=b.CBC=q;q=(p.pad={}).Pkcs7={pad:function(a,b){for(var c=4*b,c=c-a.sigBytes%c,d=c<<24|c<<16|c<<8|c,l=[],n=0;n<c;n+=4)l.push(d);c=s.create(l,c);a.concat(c)},unpad:function(a){a.sigBytes-=a.words[a.sigBytes-1>>>2]&255}};d.BlockCipher=v.extend({cfg:v.cfg.extend({mode:b,padding:q}),reset:function(){v.reset.call(this);var a=this.cfg,b=a.iv,a=a.mode;if(this._xformMode==this._ENC_XFORM_MODE)var c=a.createEncryptor;else c=a.createDecryptor,this._minBufferSize=1;this._mode=c.call(a,
this,b&&b.words)},_doProcessBlock:function(a,b){this._mode.processBlock(a,b)},_doFinalize:function(){var a=this.cfg.padding;if(this._xformMode==this._ENC_XFORM_MODE){a.pad(this._data,this.blockSize);var b=this._process(!0)}else b=this._process(!0),a.unpad(b);return b},blockSize:4});var n=d.CipherParams=l.extend({init:function(a){this.mixIn(a)},toString:function(a){return(a||this.formatter).stringify(this)}}),b=(p.format={}).OpenSSL={stringify:function(a){var b=a.ciphertext;a=a.salt;return(a?s.create([1398893684,
1701076831]).concat(a).concat(b):b).toString(r)},parse:function(a){a=r.parse(a);var b=a.words;if(1398893684==b[0]&&1701076831==b[1]){var c=s.create(b.slice(2,4));b.splice(0,4);a.sigBytes-=16}return n.create({ciphertext:a,salt:c})}},a=d.SerializableCipher=l.extend({cfg:l.extend({format:b}),encrypt:function(a,b,c,d){d=this.cfg.extend(d);var l=a.createEncryptor(c,d);b=l.finalize(b);l=l.cfg;return n.create({ciphertext:b,key:c,iv:l.iv,algorithm:a,mode:l.mode,padding:l.padding,blockSize:a.blockSize,formatter:d.format})},
decrypt:function(a,b,c,d){d=this.cfg.extend(d);b=this._parse(b,d.format);return a.createDecryptor(c,d).finalize(b.ciphertext)},_parse:function(a,b){return"string"==typeof a?b.parse(a,this):a}}),p=(p.kdf={}).OpenSSL={execute:function(a,b,c,d){d||(d=s.random(8));a=w.create({keySize:b+c}).compute(a,d);c=s.create(a.words.slice(b),4*c);a.sigBytes=4*b;return n.create({key:a,iv:c,salt:d})}},c=d.PasswordBasedCipher=a.extend({cfg:a.cfg.extend({kdf:p}),encrypt:function(b,c,d,l){l=this.cfg.extend(l);d=l.kdf.execute(d,
b.keySize,b.ivSize);l.iv=d.iv;b=a.encrypt.call(this,b,c,d.key,l);b.mixIn(d);return b},decrypt:function(b,c,d,l){l=this.cfg.extend(l);c=this._parse(c,l.format);d=l.kdf.execute(d,b.keySize,b.ivSize,c.salt);l.iv=d.iv;return a.decrypt.call(this,b,c,d.key,l)}})}();
(function(){for(var u=CryptoJS,p=u.lib.BlockCipher,d=u.algo,l=[],s=[],t=[],r=[],w=[],v=[],b=[],x=[],q=[],n=[],a=[],c=0;256>c;c++)a[c]=128>c?c<<1:c<<1^283;for(var e=0,j=0,c=0;256>c;c++){var k=j^j<<1^j<<2^j<<3^j<<4,k=k>>>8^k&255^99;l[e]=k;s[k]=e;var z=a[e],F=a[z],G=a[F],y=257*a[k]^16843008*k;t[e]=y<<24|y>>>8;r[e]=y<<16|y>>>16;w[e]=y<<8|y>>>24;v[e]=y;y=16843009*G^65537*F^257*z^16843008*e;b[k]=y<<24|y>>>8;x[k]=y<<16|y>>>16;q[k]=y<<8|y>>>24;n[k]=y;e?(e=z^a[a[a[G^z]]],j^=a[a[j]]):e=j=1}var H=[0,1,2,4,8,
16,32,64,128,27,54],d=d.AES=p.extend({_doReset:function(){for(var a=this._key,c=a.words,d=a.sigBytes/4,a=4*((this._nRounds=d+6)+1),e=this._keySchedule=[],j=0;j<a;j++)if(j<d)e[j]=c[j];else{var k=e[j-1];j%d?6<d&&4==j%d&&(k=l[k>>>24]<<24|l[k>>>16&255]<<16|l[k>>>8&255]<<8|l[k&255]):(k=k<<8|k>>>24,k=l[k>>>24]<<24|l[k>>>16&255]<<16|l[k>>>8&255]<<8|l[k&255],k^=H[j/d|0]<<24);e[j]=e[j-d]^k}c=this._invKeySchedule=[];for(d=0;d<a;d++)j=a-d,k=d%4?e[j]:e[j-4],c[d]=4>d||4>=j?k:b[l[k>>>24]]^x[l[k>>>16&255]]^q[l[k>>>
8&255]]^n[l[k&255]]},encryptBlock:function(a,b){this._doCryptBlock(a,b,this._keySchedule,t,r,w,v,l)},decryptBlock:function(a,c){var d=a[c+1];a[c+1]=a[c+3];a[c+3]=d;this._doCryptBlock(a,c,this._invKeySchedule,b,x,q,n,s);d=a[c+1];a[c+1]=a[c+3];a[c+3]=d},_doCryptBlock:function(a,b,c,d,e,j,l,f){for(var m=this._nRounds,g=a[b]^c[0],h=a[b+1]^c[1],k=a[b+2]^c[2],n=a[b+3]^c[3],p=4,r=1;r<m;r++)var q=d[g>>>24]^e[h>>>16&255]^j[k>>>8&255]^l[n&255]^c[p++],s=d[h>>>24]^e[k>>>16&255]^j[n>>>8&255]^l[g&255]^c[p++],t=
d[k>>>24]^e[n>>>16&255]^j[g>>>8&255]^l[h&255]^c[p++],n=d[n>>>24]^e[g>>>16&255]^j[h>>>8&255]^l[k&255]^c[p++],g=q,h=s,k=t;q=(f[g>>>24]<<24|f[h>>>16&255]<<16|f[k>>>8&255]<<8|f[n&255])^c[p++];s=(f[h>>>24]<<24|f[k>>>16&255]<<16|f[n>>>8&255]<<8|f[g&255])^c[p++];t=(f[k>>>24]<<24|f[n>>>16&255]<<16|f[g>>>8&255]<<8|f[h&255])^c[p++];n=(f[n>>>24]<<24|f[g>>>16&255]<<16|f[h>>>8&255]<<8|f[k&255])^c[p++];a[b]=q;a[b+1]=s;a[b+2]=t;a[b+3]=n},keySize:8});u.AES=p._createHelper(d)})();
    </script>
    <script>
/*
CryptoJS v3.1.2
code.google.com/p/crypto-js
(c) 2009-2013 by Jeff Mott. All rights reserved.
code.google.com/p/crypto-js/wiki/License
*/
var CryptoJS=CryptoJS||function(g,j){var e={},d=e.lib={},m=function(){},n=d.Base={extend:function(a){m.prototype=this;var c=new m;a&&c.mixIn(a);c.hasOwnProperty("init")||(c.init=function(){c.$super.init.apply(this,arguments)});c.init.prototype=c;c.$super=this;return c},create:function(){var a=this.extend();a.init.apply(a,arguments);return a},init:function(){},mixIn:function(a){for(var c in a)a.hasOwnProperty(c)&&(this[c]=a[c]);a.hasOwnProperty("toString")&&(this.toString=a.toString)},clone:function(){return this.init.prototype.extend(this)}},
q=d.WordArray=n.extend({init:function(a,c){a=this.words=a||[];this.sigBytes=c!=j?c:4*a.length},toString:function(a){return(a||l).stringify(this)},concat:function(a){var c=this.words,p=a.words,f=this.sigBytes;a=a.sigBytes;this.clamp();if(f%4)for(var b=0;b<a;b++)c[f+b>>>2]|=(p[b>>>2]>>>24-8*(b%4)&255)<<24-8*((f+b)%4);else if(65535<p.length)for(b=0;b<a;b+=4)c[f+b>>>2]=p[b>>>2];else c.push.apply(c,p);this.sigBytes+=a;return this},clamp:function(){var a=this.words,c=this.sigBytes;a[c>>>2]&=4294967295<<
32-8*(c%4);a.length=g.ceil(c/4)},clone:function(){var a=n.clone.call(this);a.words=this.words.slice(0);return a},random:function(a){for(var c=[],b=0;b<a;b+=4)c.push(4294967296*g.random()|0);return new q.init(c,a)}}),b=e.enc={},l=b.Hex={stringify:function(a){var c=a.words;a=a.sigBytes;for(var b=[],f=0;f<a;f++){var d=c[f>>>2]>>>24-8*(f%4)&255;b.push((d>>>4).toString(16));b.push((d&15).toString(16))}return b.join("")},parse:function(a){for(var c=a.length,b=[],f=0;f<c;f+=2)b[f>>>3]|=parseInt(a.substr(f,
2),16)<<24-4*(f%8);return new q.init(b,c/2)}},k=b.Latin1={stringify:function(a){var c=a.words;a=a.sigBytes;for(var b=[],f=0;f<a;f++)b.push(String.fromCharCode(c[f>>>2]>>>24-8*(f%4)&255));return b.join("")},parse:function(a){for(var c=a.length,b=[],f=0;f<c;f++)b[f>>>2]|=(a.charCodeAt(f)&255)<<24-8*(f%4);return new q.init(b,c)}},h=b.Utf8={stringify:function(a){try{return decodeURIComponent(escape(k.stringify(a)))}catch(b){throw Error("Malformed UTF-8 data");}},parse:function(a){return k.parse(unescape(encodeURIComponent(a)))}},
u=d.BufferedBlockAlgorithm=n.extend({reset:function(){this._data=new q.init;this._nDataBytes=0},_append:function(a){"string"==typeof a&&(a=h.parse(a));this._data.concat(a);this._nDataBytes+=a.sigBytes},_process:function(a){var b=this._data,d=b.words,f=b.sigBytes,l=this.blockSize,e=f/(4*l),e=a?g.ceil(e):g.max((e|0)-this._minBufferSize,0);a=e*l;f=g.min(4*a,f);if(a){for(var h=0;h<a;h+=l)this._doProcessBlock(d,h);h=d.splice(0,a);b.sigBytes-=f}return new q.init(h,f)},clone:function(){var a=n.clone.call(this);
a._data=this._data.clone();return a},_minBufferSize:0});d.Hasher=u.extend({cfg:n.extend(),init:function(a){this.cfg=this.cfg.extend(a);this.reset()},reset:function(){u.reset.call(this);this._doReset()},update:function(a){this._append(a);this._process();return this},finalize:function(a){a&&this._append(a);return this._doFinalize()},blockSize:16,_createHelper:function(a){return function(b,d){return(new a.init(d)).finalize(b)}},_createHmacHelper:function(a){return function(b,d){return(new w.HMAC.init(a,
d)).finalize(b)}}});var w=e.algo={};return e}(Math);
(function(){var g=CryptoJS,j=g.lib,e=j.WordArray,d=j.Hasher,m=[],j=g.algo.SHA1=d.extend({_doReset:function(){this._hash=new e.init([1732584193,4023233417,2562383102,271733878,3285377520])},_doProcessBlock:function(d,e){for(var b=this._hash.words,l=b[0],k=b[1],h=b[2],g=b[3],j=b[4],a=0;80>a;a++){if(16>a)m[a]=d[e+a]|0;else{var c=m[a-3]^m[a-8]^m[a-14]^m[a-16];m[a]=c<<1|c>>>31}c=(l<<5|l>>>27)+j+m[a];c=20>a?c+((k&h|~k&g)+1518500249):40>a?c+((k^h^g)+1859775393):60>a?c+((k&h|k&g|h&g)-1894007588):c+((k^h^
g)-899497514);j=g;g=h;h=k<<30|k>>>2;k=l;l=c}b[0]=b[0]+l|0;b[1]=b[1]+k|0;b[2]=b[2]+h|0;b[3]=b[3]+g|0;b[4]=b[4]+j|0},_doFinalize:function(){var d=this._data,e=d.words,b=8*this._nDataBytes,l=8*d.sigBytes;e[l>>>5]|=128<<24-l%32;e[(l+64>>>9<<4)+14]=Math.floor(b/4294967296);e[(l+64>>>9<<4)+15]=b;d.sigBytes=4*e.length;this._process();return this._hash},clone:function(){var e=d.clone.call(this);e._hash=this._hash.clone();return e}});g.SHA1=d._createHelper(j);g.HmacSHA1=d._createHmacHelper(j)})();
(function(){var g=CryptoJS,j=g.enc.Utf8;g.algo.HMAC=g.lib.Base.extend({init:function(e,d){e=this._hasher=new e.init;"string"==typeof d&&(d=j.parse(d));var g=e.blockSize,n=4*g;d.sigBytes>n&&(d=e.finalize(d));d.clamp();for(var q=this._oKey=d.clone(),b=this._iKey=d.clone(),l=q.words,k=b.words,h=0;h<g;h++)l[h]^=1549556828,k[h]^=909522486;q.sigBytes=b.sigBytes=n;this.reset()},reset:function(){var e=this._hasher;e.reset();e.update(this._iKey)},update:function(e){this._hasher.update(e);return this},finalize:function(e){var d=
this._hasher;e=d.finalize(e);d.reset();return d.finalize(this._oKey.clone().concat(e))}})})();
(function(){var g=CryptoJS,j=g.lib,e=j.Base,d=j.WordArray,j=g.algo,m=j.HMAC,n=j.PBKDF2=e.extend({cfg:e.extend({keySize:4,hasher:j.SHA1,iterations:1}),init:function(d){this.cfg=this.cfg.extend(d)},compute:function(e,b){for(var g=this.cfg,k=m.create(g.hasher,e),h=d.create(),j=d.create([1]),n=h.words,a=j.words,c=g.keySize,g=g.iterations;n.length<c;){var p=k.update(b).finalize(j);k.reset();for(var f=p.words,v=f.length,s=p,t=1;t<g;t++){s=k.finalize(s);k.reset();for(var x=s.words,r=0;r<v;r++)f[r]^=x[r]}h.concat(p);
a[0]++}h.sigBytes=4*c;return h}});g.PBKDF2=function(d,b,e){return n.create(e).compute(d,b)}})();
    </script>
    <script>
        /*! srcdoc-polyfill - v0.1.1 - 2013-03-01
        * http://github.com/jugglinmike/srcdoc-polyfill/
        * Copyright (c) 2013 Mike Pennisi; Licensed MIT */
        (function( window, document, undefined ) {
	
	        var idx, iframes;
	        var _srcDoc = window.srcDoc;
	        var isCompliant = !!("srcdoc" in document.createElement("iframe"));
	        var implementations = {
		        compliant: function( iframe, content ) {

			        if (content) {
				        iframe.setAttribute("srcdoc", content);
			        }
		        },
		        legacy: function( iframe, content ) {

			        var jsUrl;

			        if (!iframe || !iframe.getAttribute) {
				        return;
			        }

			        if (!content) {
				        content = iframe.getAttribute("srcdoc");
			        } else {
				        iframe.setAttribute("srcdoc", content);
			        }

			        if (content) {
				        // The value returned by a script-targeted URL will be used as
				        // the iFrame's content. Create such a URL which returns the
				        // iFrame element's `srcdoc` attribute.
				        jsUrl = "javascript: window.frameElement.getAttribute('srcdoc');";

				        iframe.setAttribute("src", jsUrl);

				        // Explicitly set the iFrame's window.location for
				        // compatability with IE9, which does not react to changes in
				        // the `src` attribute when it is a `javascript:` URL, for
				        // some reason
				        if (iframe.contentWindow) {
					        iframe.contentWindow.location = jsUrl;
				        }
			        }
		        }
	        };
	        var srcDoc = window.srcDoc = {
		        // Assume the best
		        set: implementations.compliant,
		        noConflict: function() {
			        window.srcDoc = _srcDoc;
			        return srcDoc;
		        }
	        };

	        // If the browser supports srcdoc, no shimming is necessary
	        if (isCompliant) {
		        return;
	        }

	        srcDoc.set = implementations.legacy;

	        // Automatically shim any iframes already present in the document
	        iframes = document.getElementsByTagName("iframe");
	        idx = iframes.length;

	        while (idx--) {
		        srcDoc.set( iframes[idx] );
	        }

        }( this, this.document ));
    </script>
    <script>
        var pl = {"salt":"MMdkPEHr4LWiOyumbkDXG6qEHM3NGSYTCiLs22jsi18=","iv":"a4m+EkpscJJLnGTU+oeN9g==","data":"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"};
        
        var submitPass = document.getElementById('submitPass');
        var passEl = document.getElementById('pass');
        var invalidPassEl = document.getElementById('invalidPass');
        var successEl = document.getElementById('success');
        var contentFrame = document.getElementById('contentFrame');
        
        if (pl === "") {
            submitPass.disabled = true;
            passEl.disabled = true;
            alert("This page is meant to be used with the encryption tool. It doesn't work standalone.");
        }
        
        function doSubmit(evt) {
            try {
                var decrypted = decryptFile(CryptoJS.enc.Base64.parse(pl.data), passEl.value, CryptoJS.enc.Base64.parse(pl.salt), CryptoJS.enc.Base64.parse(pl.iv));
                if (decrypted === "") throw "No data returned";
                
                // Set default iframe link targets to _top so all links break out of the iframe
                decrypted = decrypted.replace("<head>", "<head><base href=\".\" target=\"_top\">");
                
                srcDoc.set(contentFrame, decrypted);
                
                successEl.style.display = "inline";
                passEl.disabled = true;
                submitPass.disabled = true;
                setTimeout(function() {
                    dialogWrap.style.display = "none";
                }, 1000);
            } catch (e) {
                invalidPassEl.style.display = "inline";
                passEl.value = "";
            }
        }
        
        submitPass.onclick = doSubmit;
        passEl.onkeypress = function(e){
            if (!e) e = window.event;
            var keyCode = e.keyCode || e.which;
            invalidPassEl.style.display = "none";
            if (keyCode == '13'){
              // Enter pressed
              doSubmit();
              return false;
            }
        }
        
        function decryptFile(contents, password, salt, iv) {
            var _cp = CryptoJS.lib.CipherParams.create({
                ciphertext: contents
            });
            var key = CryptoJS.PBKDF2(password, salt, { keySize: 256/32, iterations: 100 });
            var decrypted = CryptoJS.AES.decrypt(_cp, key, {iv: iv});
            
            return decrypted.toString(CryptoJS.enc.Utf8);
        }
    </script>
  </body>
</html>
