---
ms.Toctitle: Preview features
title: "Office 365 プラットフォームの開発者向けプレビュー機能"
description: "Office 365 のプレビュー機能および API は、まだ運用コード用には最終処理されていませんが、プラットフォームに組み込もうとしているものの一部は早い段階で確認できます。"
ms.ContentId: 4bbbd683-c12a-449a-9215-823ce7ffd8de
ms.date: November 17, 2015

---
[!INCLUDE [Add the O365API repo styles](../includes/controls/addo365apistyles.xml)]



# Office 365 プラットフォームの開発者向けプレビュー機能

_**適用対象:**Office 365_

    
マイクロソフトでは Office 365 の開発プラットフォームを改善および拡張するよう常に努力しています。 Office 365 の開発者向け機能に、いくつかのプレビュー バージョンが追加されました。

##Office 365 のプレビュー機能とは

Office 365 のプレビュー機能とは、機能および動作のフィードバックを得るために、開発者向けに早期に提供している開発中の機能および APIです。プレビュー機能は、最終処理までに変更される場合があります。それらを使用して記述されたコードを破棄する変更がある場合もあります。このため、Office 365 でプレビューとしてマークされている機能および API は運用コードでは使用しないようにする必要があります。

これらの機能は現在プレビュー中で、運用コードで使用するにはまだ最終処理されていません。将来プラットフォームに組み込む機能を早い段階で確認し、それに対するユーザーの考えを得るためのものです。

##Office 365 API のプレビュー機能

現在プレビュー中の Office 365 API の開発スタックを、次の図に紫色でフォーマットして示します。

<svg xmlns:xlink="http://www.w3.org/1999/xlink" height="689px" width="600px">

<ph id="ph1">&lt;style type="text/css"&gt;</ph> .st0{fill:#737373;} .st1{fill:#FFFFFF;} .st2{font-family:'Arial';} .st3{font-size:11px;} .st4{fill:#DCD3E8;} .st5{fill:#68BD45;} .st6{fill:#C3E5B5;} .st7{fill:#A4A4A4;} .st8{fill:#00ABEC;} .st9{fill:#D8D8D8;} .st10{fill:#2273B9;} .st11{fill:#B7E0FF;} .st12{fill:#623997;} .st13{fill:#8EDAF7;} .st14{clip-path:url(#SVGID_2_);} .st15{clip-path:url(#SVGID_4_);fill:#68BD45;} .st16{clip-path:url(#SVGID_4_);fill:#FFFFFF;} .st17{clip-path:url(#SVGID_6_);} .st18{clip-path:url(#SVGID_8_);fill:#68BD45;} .st19{clip-path:url(#SVGID_8_);fill:#FFFFFF;} .st20{fill:#2372BA;} .st21{fill-rule:evenodd;clip-rule:evenodd;fill:#FFFFFF;} .st22{fill:#FFFFFF;stroke:#2273B9;stroke-miterlimit:10;} .st23{fill:none;stroke:#DA4026;stroke-width:2;stroke-miterlimit:10;} .st24{fill:none;stroke:#DA4026;stroke-width:2;stroke-miterlimit:10;stroke-dasharray:11.9529,11.9529;} .st25{fill:none;stroke:#DA4026;stroke-width:2;stroke-miterlimit:10;stroke-dasharray:12.1257,12.1257;} .st26{fill:#505050;} .st27{fill:#DA4026;} .st28{fill:none;stroke:#505050;stroke-width:4;stroke-miterlimit:10;} .st29{font-size:21px;} .st30{fill:#7F1D1D;stroke:#2273B9;stroke-width:4;stroke-miterlimit:10;} .st31{fill:#7F1D1D;stroke:#A4A4A4;stroke-width:4;stroke-miterlimit:10;} .st32{font-size:14px;} .st33{font-family:'Arial';} .st34{font-size:9px;} .st35{fill:#231F20;} .st36{fill:#F25022;} .st37{fill:#7FBA00;} .st38{fill:#00A4EF;} .st39{fill:#FFB900;} .st40{fill:none;stroke:#682A7A;stroke-width:2;stroke-miterlimit:10;} .st41{fill:none;stroke:#682A7A;stroke-width:2;stroke-miterlimit:10;stroke-dasharray:11.9529,11.9529;} .st42{fill:none;stroke:#682A7A;stroke-width:2;stroke-miterlimit:10;stroke-dasharray:13.1178,13.1178;} .st43{fill:none;stroke:#2273B9;stroke-width:4;stroke-miterlimit:10;} .st44{clip-path:url(#SVGID_10_);} .st45{clip-path:url(#SVGID_12_);fill:#FFFFFF;} .st46{clip-path:url(#SVGID_12_);fill:#00ABEC;} .st47{clip-path:url(#SVGID_12_);fill:#FFFFFF;stroke:#00ABEC;stroke-miterlimit:10;} .st48{clip-path:url(#SVGID_14_);fill:#FFFFFF;} .st49{clip-path:url(#SVGID_14_);} .st50{clip-path:url(#SVGID_16_);fill:#2273B9;} .st51{clip-path:url(#SVGID_18_);} .st52{clip-path:url(#SVGID_20_);fill:#FFFFFF;} .st53{clip-path:url(#SVGID_20_);fill:#00ABEC;} .st54{clip-path:url(#SVGID_22_);} .st55{clip-path:url(#SVGID_24_);fill:#FFFFFF;} .st56{clip-path:url(#SVGID_24_);fill:#00ABEC;} .st57{clip-path:url(#SVGID_26_);} .st58{clip-path:url(#SVGID_28_);fill:#FFFFFF;} .st59{clip-path:url(#SVGID_28_);fill:#2273B9;} .st60{clip-path:url(#SVGID_30_);fill:#FFFFFF;} .st61{clip-path:url(#SVGID_30_);fill:#2273B9;} .st62{clip-path:url(#SVGID_32_);fill:#FFFFFF;} .st63{clip-path:url(#SVGID_32_);fill:#2273B9;} .st64{fill:#FFFFFF;stroke:#2273B9;stroke-width:2;stroke-miterlimit:10;} .st65{font-family:'Arial';} .st66{font-size:12px;} .st67{fill:none;stroke:#2273B9;stroke-width:2;stroke-miterlimit:10;} .st68{clip-path:url(#SVGID_34_);} .st69{clip-path:url(#SVGID_36_);fill:#FFFFFF;} .st70{clip-path:url(#SVGID_36_);fill:#2273B9;} .st71{fill-rule:evenodd;clip-rule:evenodd;fill:#00ABEC;stroke:#FFFFFF;stroke-width:1.5;stroke-miterlimit:10;} .st72{fill:none;stroke:#FFFFFF;stroke-width:1.5;stroke-miterlimit:10;} .st73{clip-path:url(#SVGID_38_);fill:#FFFFFF;} .st74{fill:none;} .st75{font-family:'Arial';} .st76{fill-rule:evenodd;clip-rule:evenodd;fill:#68BD45;} <ph id="ph2">&lt;/style&gt;
&lt;g id="Gray_boxes"&gt;
    &lt;rect x="5" y="11" class="st0" width="101" height="132"/&gt;
    &lt;rect x="5" y="154" class="st0" width="101" height="112"/&gt;
    &lt;rect x="5" y="279" class="st0" width="101" height="161"/&gt;
    &lt;rect x="5" y="450" class="st0" width="101" height="226"/&gt;
    &lt;text transform="matrix(1 0 0 1 19 207.6622)"&gt;&lt;tspan x="0" y="0" class="st1 st2 st3"&gt;</ph>Development  <ph id="ph3">&lt;/tspan&gt;&lt;tspan x="0" y="13.2" class="st1 st2 st3"&gt;</ph>Environment<ph id="ph4">&lt;/tspan&gt;&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 19 81.1622)" class="st1 st2 st3"&gt;</ph>Solution<ph id="ph5">&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 19 359.6149)" class="st1 st2 st3"&gt;</ph>Authentication<ph id="ph6">&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 19 559.1622)" class="st1 st2 st3"&gt;</ph>Data<ph id="ph7">&lt;/text&gt;
&lt;/g&gt;
&lt;g id="ENCLOSURES"&gt;
&lt;/g&gt;
&lt;g id="LINES__x2F__ARROWS"&gt;
    &lt;rect x="115" y="311" class="st4" width="478" height="129"/&gt;
    &lt;rect x="115" y="11" class="st5" width="478" height="39"/&gt;
    &lt;rect x="115" y="48" class="st6" width="478.2" height="95.2"/&gt;
    &lt;rect x="123" y="57" class="st7" width="463" height="33"/&gt;
    &lt;rect x="123" y="98" class="st8" width="230" height="33.8"/&gt;
    &lt;rect x="357" y="98" class="st7" width="228.7" height="33.8"/&gt;
    &lt;rect x="115" y="154" class="st9" width="478.2" height="113"/&gt;
    &lt;rect x="123" y="168" class="st7" width="111" height="34"/&gt;
    &lt;rect x="241" y="168" class="st7" width="112" height="34"/&gt;
    &lt;rect x="357" y="168" class="st7" width="111" height="34"/&gt;
    &lt;rect x="473" y="168" class="st7" width="113" height="34"/&gt;
    &lt;rect x="123" y="210" class="st10" width="111" height="33"/&gt;
    &lt;rect x="241" y="210" class="st10" width="112" height="33"/&gt;
    &lt;rect x="357" y="210" class="st10" width="111" height="33"/&gt;
    &lt;rect x="123" y="361" class="st8" width="224.8" height="38"/&gt;
    &lt;rect x="354.1" y="361" class="st11" width="231.9" height="38"/&gt;
    &lt;rect x="123.2" y="322" class="st12" width="462.8" height="28"/&gt;
    &lt;rect x="123" y="407" class="st7" width="463" height="20"/&gt;
    &lt;rect x="123" y="505" class="st10" width="463" height="24"/&gt;
    &lt;rect x="123" y="539" class="st10" width="39" height="109.8"/&gt;
    &lt;rect x="255" y="539" class="st10" width="39" height="109.8"/&gt;
    &lt;rect x="429" y="539" class="st8" width="39" height="109.8"/&gt;
    &lt;rect x="162" y="539" class="st11" width="87" height="109.8"/&gt;
    &lt;rect x="294" y="539" class="st11" width="128" height="109.8"/&gt;
    &lt;rect x="468" y="539" class="st13" width="52" height="109.8"/&gt;
    &lt;rect x="526" y="539" class="st12" width="60" height="121.5"/&gt;
    &lt;rect x="123" y="655" class="st12" width="463" height="10"/&gt;
    &lt;text transform="matrix(1 0 0 1 288.1081 70.4953)"&gt;&lt;tspan x="0" y="0" class="st2 st3"&gt;</ph>Your choice of technology<ph id="ph8">&lt;/tspan&gt;&lt;tspan x="-20.6" y="13.2" class="st2 st3"&gt;</ph>(.NET, JavaScript, HTML, Ruby, etc.)<ph id="ph9">&lt;/tspan&gt;&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 437.4897 111.162)"&gt;&lt;tspan x="0" y="0" class="st2 st3"&gt;</ph>Other hosting<ph id="ph10">&lt;/tspan&gt;&lt;tspan x="-3.6" y="13.2" class="st2 st3"&gt;</ph>(IIS, LAMP, etc.)<ph id="ph11">&lt;/tspan&gt;&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 162.6362 188.4955)" class="st2 st3"&gt;</ph>XCode<ph id="ph12">&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 272.5954 182.162)"&gt;&lt;tspan x="0" y="0" class="st2 st3"&gt;</ph>Eclipse or <ph id="ph13">&lt;/tspan&gt;&lt;tspan x="-13.7" y="13.2" class="st2 st3"&gt;</ph>Android Studio<ph id="ph14">&lt;/tspan&gt;&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 379.4771 188.4953)" class="st2 st3"&gt;</ph>Visual Studio<ph id="ph15">&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 517.7487 188.4955)" class="st2 st3"&gt;</ph>REST<ph id="ph16">&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 170.2235 222.8288)"&gt;&lt;tspan x="0" y="0" class="st1 st2 st3"&gt;</ph>iOS <ph id="ph17">&lt;/tspan&gt;&lt;tspan x="-27.8" y="13.2" class="st1 st2 st3"&gt;</ph>Office 365 SDK<ph id="ph18">&lt;/tspan&gt;&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 276.0621 222.8288)"&gt;&lt;tspan x="0" y="0" class="st1 st2 st3"&gt;</ph>Android <ph id="ph19">&lt;/tspan&gt;&lt;tspan x="-16.6" y="13.2" class="st1 st2 st3"&gt;</ph>Office 365 SDK<ph id="ph20">&lt;/tspan&gt;&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 381.1005 222.829)"&gt;&lt;tspan x="0" y="0" class="st1 st2 st3"&gt;</ph>Visual Studio <ph id="ph21">&lt;/tspan&gt;&lt;tspan x="-4.7" y="13.2" class="st1 st2 st3"&gt;</ph>Office 365 SDK<ph id="ph22">&lt;/tspan&gt;&lt;/text&gt;</ph><bpt id="p1">
    &lt;g&gt;</bpt><ph id="ph23">
        &lt;path class="st5" d="M300.5,42.3c-1.4,0-2.5-0.6-2.9-1.5c-0.3-0.7-0.2-1.5,0.4-2.1c0.3-0.3,0.8-0.7,1.3-1.2
            c0.4-0.4,0.8-0.8,1.2-1.1V20.9c0-1.7,1.2-3,2.9-3h26.9c1.7,0,3,1.3,3,3v15.6c0.3,0.3,0.8,0.7,1.2,1.1c0.5,0.4,0.9,0.8,1.2,1.1
            c0.6,0.6,0.8,1.4,0.5,2.1c-0.4,1-1.5,1.5-3,1.5H300.5z"/&gt;
        &lt;path class="st1" d="M334.7,39.5c-0.7-0.7-2.1-1.9-2.7-2.6V20.9c0-1-0.7-1.8-1.8-1.8h-26.9c-1,0-1.7,0.7-1.7,1.8V37
            c-0.7,0.7-2.1,1.9-2.8,2.6c-0.6,0.7,0,1.6,1.7,1.6h32.6C334.7,41.2,335.4,40.2,334.7,39.5z M319.9,40h-6.2c-0.2,0-0.3-0.2-0.2-0.3
            l1-1.4c0-0.1,0.1-0.1,0.2-0.1h4.2c0.1,0,0.1,0,0.2,0.1l1,1.4C320.1,39.8,320,40,319.9,40z M330.2,36.4h-26.8V20.9h26.8V36.4z"/&gt;</ph><ept id="p1">
    &lt;/g&gt;</ept><ph id="ph24">
    &lt;g&gt;
        &lt;g&gt;</ph><bpt id="p2">
            &lt;defs&gt;</bpt><ph id="ph25">
                &lt;rect id="SVGID_1_" x="233.5" y="13.3" width="31.7" height="33.6"/&gt;</ph><ept id="p2">
            &lt;/defs&gt;</ept><ph id="ph26">
            &lt;clipPath id="SVGID_2_"&gt;
                &lt;use xlink:href="#SVGID_1_"  style="overflow:visible;"/&gt;
            &lt;/clipPath&gt;
            &lt;g class="st14"&gt;</ph><bpt id="p3">
                &lt;defs&gt;</bpt><ph id="ph27">
                    &lt;rect id="SVGID_3_" x="233.5" y="13.3" width="31.7" height="33.6"/&gt;</ph><ept id="p3">
                &lt;/defs&gt;</ept><ph id="ph28">
                &lt;clipPath id="SVGID_4_"&gt;
                    &lt;use xlink:href="#SVGID_3_"  style="overflow:visible;"/&gt;
                &lt;/clipPath&gt;
                &lt;path class="st15" d="M244.2,44.3c-1.3,0-2.4-1.2-2.4-2.6V18.5c0-1.4,1.1-2.6,2.4-2.6h10.7c1.3,0,2.4,1.2,2.4,2.6v23.3
                    c0,1.4-1.1,2.6-2.4,2.6H244.2z"/&gt;
                &lt;path class="st16" d="M254.9,16.9h-10.7c-0.8,0-1.5,0.7-1.5,1.6v23.3c0,0.9,0.7,1.6,1.5,1.6h10.7c0.8,0,1.5-0.7,1.5-1.6V18.5
                    C256.4,17.6,255.7,16.9,254.9,16.9 M250.5,41.7h-1.9c-0.4,0-0.7-0.3-0.7-0.8s0.3-0.8,0.7-0.8h1.9c0.4,0,0.7,0.3,0.7,0.8
                    S250.9,41.7,250.5,41.7 M254.9,38.6h-10.7V20h10.7V38.6z"/&gt;
            &lt;/g&gt;
        &lt;/g&gt;
    &lt;/g&gt;
    &lt;g&gt;
        &lt;g&gt;</ph><bpt id="p4">
            &lt;defs&gt;</bpt><ph id="ph29">
                &lt;rect id="SVGID_5_" x="259.9" y="10.9" width="37.5" height="37.5"/&gt;</ph><ept id="p4">
            &lt;/defs&gt;</ept><ph id="ph30">
            &lt;clipPath id="SVGID_6_"&gt;
                &lt;use xlink:href="#SVGID_5_"  style="overflow:visible;"/&gt;
            &lt;/clipPath&gt;
            &lt;g class="st17"&gt;</ph><bpt id="p5">
                &lt;defs&gt;</bpt><ph id="ph31">
                    &lt;rect id="SVGID_7_" x="259.9" y="10.9" width="37.5" height="37.5"/&gt;</ph><ept id="p5">
                &lt;/defs&gt;</ept><ph id="ph32">
                &lt;clipPath id="SVGID_8_"&gt;
                    &lt;use xlink:href="#SVGID_7_"  style="overflow:visible;"/&gt;
                &lt;/clipPath&gt;
                &lt;path class="st18" d="M265.1,42.6c-1.6,0-2.9-1.3-2.9-2.8V20c0-1.5,1.3-2.8,2.9-2.8h27.2c1.5,0,2.8,1.3,2.8,2.8v19.8
                    c0,1.5-1.3,2.8-2.8,2.8H265.1z"/&gt;
                &lt;path class="st19" d="M292.8,22.4v17.4c0,0.3,0.3,0.8,0-1l-28.3-0.1c-0.3,1.8,0,1.4,0,1.1V22.4H292.8z M292.3,18.4h-27.2
                    c-0.9,0-1.7,0.8-1.7,1.7v19.8c0,0.9,0.8,1.6,1.7,1.6h27.2c0.9,0,1.6-0.7,1.6-1.7V20C293.9,19.1,293.2,18.4,292.3,18.4"/&gt;
                &lt;rect x="291.6" y="19.5" class="st19" width="1.2" height="1.2"/&gt;
                &lt;rect x="289.3" y="19.5" class="st19" width="1.2" height="1.2"/&gt;
                &lt;rect x="287" y="19.5" class="st19" width="1.2" height="1.2"/&gt;
            &lt;/g&gt;
        &lt;/g&gt;
    &lt;/g&gt;</ph><bpt id="p6">
    &lt;g&gt;</bpt><ph id="ph33">
        &lt;path class="st10" d="M289.1,549.8h-10.8v-7.4h-2.3l-17.7,3.1V572l17,3h3v-6.9h10.8c1,0,1.9-0.8,1.9-1.9v-14.6
            C291,550.6,290.1,549.8,289.1,549.8z"/&gt;</ph><bpt id="p7">
        &lt;g&gt;</bpt><ph id="ph34">
            &lt;path class="st1" d="M289.1,550.9h-11.8v-7.6l-17.9,3.2v24.7l17.9,3.1v-7.2h11.8c0.4,0,0.8-0.4,0.8-0.8v-14.6
                C289.9,551.2,289.6,550.9,289.1,550.9z M288.9,566.1h-11.6v-9.3l3.6,3.5c0.1,0.1,0.3,0.2,0.5,0.2c0.2,0,0.4-0.1,0.5-0.2l6.9-6.4
                V566.1z M288.9,552.5l-7.4,6.9l-4.2-4v-3.6h11.6V552.5z"/&gt;
            &lt;path class="st10" d="M268,562.4c-3.2-0.1-3.1-7,0.1-7.1C271.2,555.1,271.3,562.5,268,562.4 M268.1,553.1
                c-5.8,0.4-6,11.1-0.1,11.4C274.2,564.9,274.4,552.7,268.1,553.1"/&gt;</ph><ept id="p7">
        &lt;/g&gt;</ept><ept id="p6">
    &lt;/g&gt;</ept><bpt id="p8">
    &lt;g&gt;</bpt><bpt id="p9">
        &lt;g&gt;</bpt><ph id="ph35">
            &lt;path class="st10" d="M154.7,595.3c-0.2-2.2-1.6-3.7-3-4.3c-0.4-0.2-0.8-0.3-1.1-0.4l0-0.1c-0.2-2.1-1.6-4.9-4.7-6.1
                c-0.9-0.4-1.8-0.5-2.8-0.5c-2.3,0-4.4,1.1-5.8,3c-0.5-0.2-1.2-0.4-2-0.4c-0.9,0-1.8,0.2-2.7,0.7c-1.9,1-3.1,3-3.2,5.1
                c-1.6,0.5-3.6,1.8-3.6,4.9c0,2.7,2.3,5,4.9,5h3.5c0.9,0.8,2.1,1.3,3.6,1.3h16c2.5,0,3.8-2.1,3.8-4.2
                C157.5,597,155.9,595.8,154.7,595.3z"/&gt;
            &lt;path class="st1" d="M153.7,596.1c0,0,2.8,0.3,2.8,3.3c0,1.5-0.8,3.2-2.8,3.2c0,0-15,0-16,0c-3.1,0-4.2-2.1-4.2-4.2
                c0-3.6,4-3.7,4-3.7s0.3-4,3.8-4.8c3.1-0.7,4.9,0.9,5.7,2.2c0,0,1.9-1.1,4.1-0.1C152.5,592.5,153.8,593.9,153.7,596.1z"/&gt;
            &lt;path class="st1" d="M132.5,598.6c0-4,4.3-4.6,4.3-4.6s0.5-3.9,4.4-4.9c3-0.8,5.3,0.6,6.2,2.1c0,0,0.6-0.5,2.1-0.4
                c-0.1-1.3-1.1-4.1-4.1-5.3c-3.4-1.3-6.4,0.3-7.8,2.8c0,0-2.1-1.4-4.6-0.1c-1.8,0.9-2.9,3-2.6,5.1c0,0-3.6,0.3-3.6,4.1
                c0,2.1,1.9,4,3.9,4h2.7C132.7,600.4,132.5,599.4,132.5,598.6z"/&gt;</ph><ept id="p9">
        &lt;/g&gt;</ept><ept id="p8">
    &lt;/g&gt;</ept><bpt id="p10">
    &lt;g&gt;</bpt><ph id="ph36">
        &lt;path class="st20" d="M155.1,554c-1-1.7-2.4-3.1-4-4c-0.5-2.3-2.5-4-4.9-4.1v-3.3l-19.9,3.5v26.4l19.9,3.5v-3.4
            c2.4-0.1,4.4-1.8,4.9-4.1c1.7-0.9,3.1-2.3,4-4c2.4-0.5,4.2-2.7,4.2-5.2C159.3,556.6,157.5,554.5,155.1,554z M149.5,562.1
            c-0.2,0.2-0.4,0.5-0.7,0.7c-0.8-0.5-1.7-0.8-2.6-0.9v-5.5c0.9-0.1,1.9-0.4,2.6-0.9c0.3,0.2,0.5,0.4,0.7,0.7
            c-0.6,0.9-0.9,1.9-0.9,2.9C148.6,560.2,148.9,561.2,149.5,562.1z"/&gt;
        &lt;path class="st21" d="M145.8,570.9c-2,0-3.7-1.6-3.8-3.6c-2-0.9-3.6-2.5-4.5-4.4c-2-0.1-3.6-1.7-3.6-3.7c0-2,1.6-3.7,3.6-3.7
            c0.9-2,2.5-3.5,4.5-4.4c0.1-2,1.7-3.6,3.8-3.6c2,0,3.7,1.6,3.8,3.6c2,0.9,3.6,2.5,4.5,4.4c2,0.1,3.6,1.7,3.6,3.7
            c0,2-1.6,3.7-3.6,3.7c-0.9,2-2.5,3.5-4.5,4.4C149.5,569.3,147.9,570.9,145.8,570.9z M140.2,561.9c0.6,1.2,1.6,2.2,2.8,2.8
            c0.7-0.8,1.7-1.3,2.8-1.3c1.1,0,2.1,0.5,2.8,1.3c1.2-0.6,2.2-1.6,2.8-2.8c-0.8-0.7-1.3-1.7-1.3-2.8c0-1.1,0.5-2.1,1.3-2.8
            c-0.6-1.2-1.6-2.2-2.8-2.8c-0.7,0.8-1.7,1.3-2.8,1.3c-1.1,0-2.1-0.5-2.8-1.3c-1.2,0.6-2.2,1.6-2.8,2.8c0.8,0.7,1.3,1.7,1.3,2.8
            C141.5,560.2,141,561.2,140.2,561.9z"/&gt;
        &lt;path class="st20" d="M144.9,571.3c0.3,0.1,0.6,0.1,0.9,0.1c2.2,0,4-1.7,4.3-3.8c1.9-0.9,3.4-2.4,4.3-4.3c2.1-0.2,3.8-2,3.8-4.2
            c0-2.2-1.7-4-3.8-4.2c-0.9-1.8-2.5-3.4-4.3-4.3c-0.2-2.1-2.1-3.8-4.3-3.8c-0.3,0-0.6,0-0.9,0.1 M144.9,555.3
            c0.3,0.1,0.6,0.1,0.9,0.1c1.1,0,2.1-0.4,2.9-1.1c0.8,0.5,1.5,1.2,2,2c-0.7,0.8-1.1,1.8-1.1,2.9c0,1.1,0.4,2.1,1.1,2.9
            c-0.5,0.8-1.2,1.5-2,2c-0.8-0.7-1.8-1.1-2.9-1.1c-0.3,0-0.6,0-0.9,0.1V555.3z M153.9,562.4c-0.1,0-0.1,0-0.2,0
            c-0.9,2.1-2.6,3.8-4.7,4.6c0,0.1,0,0.1,0,0.2c0,1.8-1.4,3.2-3.2,3.2c-0.3,0-0.6,0-0.9-0.1v-6.2c0.3-0.1,0.6-0.1,0.9-0.1
            c1.1,0,2.1,0.6,2.7,1.4c1.6-0.7,2.9-2,3.6-3.6c-0.8-0.6-1.4-1.6-1.4-2.6s0.6-2.1,1.4-2.6c-0.7-1.6-2-2.9-3.6-3.6
            c-0.6,0.8-1.6,1.4-2.7,1.4c-0.3,0-0.6-0.1-0.9-0.1V548c0.3-0.1,0.6-0.1,0.9-0.1c1.8,0,3.2,1.4,3.2,3.2c0,0.1,0,0.1,0,0.2
            c2.1,0.9,3.8,2.5,4.7,4.6c0.1,0,0.1,0,0.2,0c1.8,0,3.2,1.4,3.2,3.2C157.2,560.9,155.7,562.4,153.9,562.4z"/&gt;
        &lt;path class="st22" d="M145.1,543.7l-17.8,3.1v24.6l17.8,3.1V543.7z"/&gt;
        &lt;path class="st10" d="M136.4,552.9c-1.5,0.1-2.9,0.9-3.3,2.4c-0.4,1.5,0.1,3,1.4,3.9c0.8,0.6,3.1,1.4,2.3,2.8
            c-0.3,0.6-1.1,0.6-1.7,0.5c-0.8-0.1-1.5-0.6-2.1-1.2c0,0.4,0,0.9,0,1.3c0,0.3,0,0.6,0,0.8c0,0.3,0.1,0.3,0.4,0.5
            c1.8,1,4.9,0.8,5.7-1.4c0.4-1,0.3-2.3-0.3-3.2c-0.6-0.9-1.6-1.4-2.5-1.9c-0.4-0.2-0.9-0.5-1-1c-0.2-0.5-0.1-1,0.3-1.3
            c0.9-0.8,2.5-0.2,3.3,0.4c0-0.6,0-1.2,0-1.9c0-0.1,0-0.3,0-0.4c0-0.2-0.1-0.1-0.3-0.2C137.8,552.9,137.1,552.8,136.4,552.9"/&gt;</ph><ept id="p10">
    &lt;/g&gt;</ept><bpt id="p11">
    &lt;g&gt;</bpt><bpt id="p12">
        &lt;g&gt;</bpt><ph id="ph37">
            &lt;polyline class="st23" points="593.2,486.3 593.2,480.3 587.2,480.3          "/&gt;
            &lt;line class="st24" x1="575.2" y1="480.3" x2="127" y2="480.3"/&gt;
            &lt;polyline class="st23" points="121,480.3 115,480.3 115,486.3            "/&gt;
            &lt;line class="st25" x1="115" y1="498.4" x2="115" y2="662.1"/&gt;
            &lt;polyline class="st23" points="115,668.2 115,674.2 121,674.2            "/&gt;
            &lt;line class="st24" x1="133" y1="674.2" x2="581.2" y2="674.2"/&gt;
            &lt;polyline class="st23" points="587.2,674.2 593.2,674.2 593.2,668.2          "/&gt;
            &lt;line class="st25" x1="593.2" y1="656" x2="593.2" y2="492.3"/&gt;</ph><ept id="p12">
        &lt;/g&gt;</ept><ept id="p11">
    &lt;/g&gt;</ept><bpt id="p13">
    &lt;g&gt;</bpt><bpt id="p14">
        &lt;g&gt;</bpt><ph id="ph38">
            &lt;path class="st1" d="M138.9,494.6c-6.2,0-11.2-5.3-11.2-11.9c0-6.4,4.8-11.6,10.8-11.8c1.9-2.3,4.5-3.6,7.4-3.7
                c1.2-6.1,6.3-10.6,12.4-10.6c4,0,7.7,2,10.1,5.4c1.1-0.4,2.3-0.5,3.5-0.5c6.8,0,12.3,5.8,12.3,13c0,0.1,0,0.1,0,0.2
                c2.8,2,4.5,5.4,4.5,9c0,5-3.2,9.4-7.9,10.7l-0.1,0l-1.5,0.3H138.9z"/&gt;
            &lt;path class="st26" d="M182,475.6c0-0.4,0.1-0.8,0.1-1.2c0-6.1-4.6-11-10.3-11c-1.5,0-3,0.3-4.3,1c-1.9-3.6-5.4-5.8-9.2-5.8
                c-5.7,0-10.3,4.7-10.7,10.7c-0.4-0.1-0.9-0.1-1.3-0.1c-2.7,0-5.2,1.4-6.8,3.8c-0.2,0-0.4,0-0.6,0c-5.1,0-9.2,4.4-9.2,9.8
                c0,5.4,4.1,9.9,9.2,9.9H179l1.3-0.3c3.8-1,6.4-4.6,6.4-8.7C186.6,480.2,184.9,477.1,182,475.6 M179.8,490.3l-1,0.2h-39.9
                c-4,0-7.2-3.5-7.2-7.9c0-4.3,3.2-7.8,7.2-7.8c0.2,0,0.3,0,0.5,0l1.2,0.1l0.6-1c1.2-1.8,3.1-2.9,5.1-2.9c0.3,0,0.7,0,1,0.1
                l2.2,0.4l0.1-2.3c0.3-4.9,4.1-8.8,8.7-8.8c3.1,0,5.9,1.8,7.5,4.8l0.9,1.7l1.8-0.9c1.1-0.5,2.2-0.8,3.4-0.8c4.6,0,8.3,4,8.3,9
                c0,0.3,0,0.7-0.1,1l-0.1,1.3l1.2,0.6c2.2,1.2,3.6,3.6,3.6,6.2C184.6,486.8,182.7,489.5,179.8,490.3"/&gt;
            &lt;polygon class="st1" points="183.1,451.6 192.7,454.1 192.7,479 183.1,481.4 166.7,475.7 166.7,457.2          "/&gt;
            &lt;polygon class="st27" points="190.7,477.4 190.7,455.7 183.2,453.7 168.7,458.7 168.7,458.7 168.7,474.4 173.7,472.5 
                173.7,459.7 183.7,457.5 183.7,476.3 168.7,474.4 183.2,479.3             "/&gt;</ph><ept id="p14">
        &lt;/g&gt;</ept><ept id="p13">
    &lt;/g&gt;</ept><bpt id="p15">
    &lt;g&gt;</bpt><bpt id="p16">
        &lt;g&gt;</bpt><ph id="ph39">
            &lt;line class="st28" x1="354.1" y1="300.4" x2="354.1" y2="275.1"/&gt;</ph><bpt id="p17">
            &lt;g&gt;</bpt><ph id="ph40">
                &lt;polygon class="st26" points="359.3,298.9 354.1,307.9 348.9,298.9               "/&gt;</ph><ept id="p17">
            &lt;/g&gt;</ept><bpt id="p18">
            &lt;g&gt;</bpt><ph id="ph41">
                &lt;polygon class="st26" points="359.3,276.7 354.1,267.7 348.9,276.7               "/&gt;</ph><ept id="p18">
            &lt;/g&gt;</ept><ept id="p16">
        &lt;/g&gt;</ept><ept id="p15">
    &lt;/g&gt;</ept><ph id="ph42">
    &lt;text transform="matrix(1 0 0 1 132.1909 33.6622)" class="st1 st2 st3"&gt;</ph>Your app<ph id="ph43">&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 340.1709 39.9591)" class="st1 st2 st29"&gt;</ph>...<ph id="ph44">&lt;/text&gt;
    &lt;line class="st30" x1="178.6" y1="240.9" x2="178.6" y2="256.7"/&gt;
    &lt;line class="st30" x1="296" y1="241.4" x2="296" y2="257.2"/&gt;
    &lt;line class="st30" x1="413" y1="240.4" x2="413" y2="256.2"/&gt;
    &lt;line class="st31" x1="529.6" y1="200.8" x2="529.6" y2="216.5"/&gt;
    &lt;text transform="matrix(1 0 0 1 197.9624 120.1622)" class="st1 st2 st32"&gt;</ph>Microsoft Azure<ph id="ph45">&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 167.2066 378.055)"&gt;&lt;tspan x="0" y="0" class="st1 st2 st3"&gt;</ph>Work or school ID (Azure AD) <ph id="ph46">&lt;/tspan&gt;&lt;tspan x="11.1" y="10.8" class="st1 st33 st34"&gt;</ph>user@domain.onmicrosoft.com<ph id="ph47">&lt;/tspan&gt;&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 419.2991 378.305)"&gt;&lt;tspan x="0" y="0" class="st35 st2 st3"&gt;</ph>Personal (Microsoft ID) <ph id="ph48">&lt;/tspan&gt;&lt;tspan x="-20.5" y="10.8" class="st35 st33 st34"&gt;</ph>user@live.com, user@hotmail.com, etc.<ph id="ph49">&lt;/tspan&gt;&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 329.192 519.9997)" class="st1 st2 st3"&gt;</ph>REST APIs<ph id="ph50">&lt;/text&gt;
    &lt;text transform="matrix(1 0 0 1 275.085 420.805)" class="st35 st2 st3"&gt;</ph>OAuth 2.0 and Open ID Connect 1.0<ph id="ph51">&lt;/text&gt;</ph><bpt id="p19">
    &lt;g&gt;</bpt><ph id="ph52">
        &lt;rect x="359.2" y="366.3" class="st36" width="13.9" height="13.9"/&gt;
        &lt;rect x="374.7" y="366.3" class="st37" width="13.9" height="13.9"/&gt;
        &lt;rect x="359.2" y="382.2" class="st38" width="13.9" height="13.9"/&gt;
        &lt;rect x="374.7" y="382.2" class="st39" width="13.9" height="13.9"/&gt;</ph><ept id="p19">
    &lt;/g&gt;</ept><bpt id="p20">
    &lt;g&gt;</bpt><bpt id="p21">
        &lt;g&gt;</bpt><ph id="ph53">
            &lt;line class="st28" x1="353.1" y1="472.8" x2="353.1" y2="447.5"/&gt;</ph><bpt id="p22">
            &lt;g&gt;</bpt><ph id="ph54">
                &lt;polygon class="st26" points="358.3,471.3 353.1,480.3 347.9,471.3               "/&gt;</ph><ept id="p22">
            &lt;/g&gt;</ept><bpt id="p23">
            &lt;g&gt;</bpt><ph id="ph55">
                &lt;polygon class="st26" points="358.3,449 353.1,440 347.9,449                 "/&gt;</ph><ept id="p23">
            &lt;/g&gt;</ept><ept id="p21">
        &lt;/g&gt;</ept><ept id="p20">
    &lt;/g&gt;</ept><bpt id="p24">
    &lt;g&gt;</bpt><bpt id="p25">
        &lt;g&gt;</bpt><ph id="ph56">
            &lt;polyline class="st40" points="593,316 593,310 587,310          "/&gt;
            &lt;line class="st41" x1="575" y1="310" x2="126.8" y2="310"/&gt;
            &lt;polyline class="st40" points="120.8,310 114.8,310 114.8,316            "/&gt;
            &lt;line class="st42" x1="114.8" y1="329.1" x2="114.8" y2="427.5"/&gt;
            &lt;polyline class="st40" points="114.8,434.1 114.8,440.1 120.8,440.1          "/&gt;
            &lt;line class="st41" x1="132.8" y1="440.1" x2="581" y2="440.1"/&gt;
            &lt;polyline class="st40" points="587,440.1 593,440.1 593,434.1            "/&gt;
            &lt;line class="st42" x1="593" y1="420.9" x2="593" y2="322.6"/&gt;</ph><ept id="p25">
        &lt;/g&gt;</ept><ept id="p24">
    &lt;/g&gt;</ept><ph id="ph57">
    &lt;path class="st1" d="M140.9,318.5c-6.2,0-11.2-5.3-11.2-11.9c0-6.4,4.8-11.6,10.8-11.8c1.9-2.3,4.5-3.6,7.4-3.7
        c1.2-6.1,6.3-10.6,12.4-10.6c4,0,7.7,2,10.1,5.4c1.1-0.4,2.3-0.5,3.5-0.5c6.8,0,12.3,5.8,12.3,13c0,0.1,0,0.1,0,0.2
        c2.8,2,4.5,5.4,4.5,9c0,5-3.2,9.4-7.9,10.7l-0.1,0l-1.5,0.3H140.9z"/&gt;
    &lt;path class="st26" d="M184,299.5c0-0.4,0.1-0.8,0.1-1.2c0-6.1-4.6-11-10.3-11c-1.5,0-3,0.3-4.3,1c-1.9-3.6-5.4-5.8-9.2-5.8
        c-5.7,0-10.3,4.7-10.7,10.7c-0.4-0.1-0.9-0.1-1.3-0.1c-2.7,0-5.2,1.4-6.8,3.8c-0.2,0-0.4,0-0.6,0c-5.1,0-9.2,4.4-9.2,9.8
        c0,5.4,4.1,9.9,9.2,9.9H181l1.3-0.3c3.8-1,6.4-4.6,6.4-8.7C188.6,304.1,186.9,301.1,184,299.5 M181.8,314.3l-1,0.2h-39.9
        c-4,0-7.2-3.5-7.2-7.9c0-4.3,3.2-7.8,7.2-7.8c0.2,0,0.3,0,0.5,0l1.2,0.1l0.6-1c1.2-1.8,3.1-2.9,5.1-2.9c0.3,0,0.7,0,1,0.1l2.2,0.4
        l0.1-2.3c0.3-4.9,4.1-8.8,8.7-8.8c3.1,0,5.9,1.8,7.5,4.8l0.9,1.7l1.8-0.9c1.1-0.5,2.2-0.8,3.4-0.8c4.6,0,8.3,4,8.3,9
        c0,0.3,0,0.7-0.1,1l-0.1,1.3l1.2,0.6c2.2,1.2,3.6,3.6,3.6,6.2C186.6,310.7,184.7,313.5,181.8,314.3"/&gt;</ph><bpt id="p26">
    &lt;g&gt;</bpt><ph id="ph58">
        &lt;rect x="168.8" y="279.6" class="st1" width="25" height="25"/&gt;
        &lt;rect x="169.8" y="280.9" class="st36" width="10.6" height="10.6"/&gt;
        &lt;rect x="181.6" y="280.9" class="st37" width="10.6" height="10.6"/&gt;
        &lt;rect x="169.8" y="293" class="st38" width="10.6" height="10.6"/&gt;
        &lt;rect x="181.6" y="293" class="st39" width="10.6" height="10.6"/&gt;</ph><ept id="p26">
    &lt;/g&gt;</ept><ph id="ph59">
    &lt;line class="st43" x1="354" y1="492.7" x2="354" y2="508.5"/&gt;
    &lt;text transform="matrix(1 0 0 1 541.6622 588.0411)"&gt;&lt;tspan x="0" y="0" class="st1 st2 st3"&gt;</ph>Office <ph id="ph60">&lt;/tspan&gt;&lt;tspan x="-0.5" y="13.2" class="st1 st2 st3"&gt;</ph>Graph <ph id="ph61">&lt;/tspan&gt;&lt;tspan x="-4.3" y="26.4" class="st1 st2 st3"&gt;</ph>insights<ph id="ph62">&lt;/tspan&gt;&lt;/text&gt;
    &lt;g&gt;
        &lt;g&gt;</ph><bpt id="p27">
            &lt;defs&gt;</bpt><ph id="ph63">
                &lt;rect id="SVGID_9_" x="431.6" y="542.4" width="34.1" height="34.1"/&gt;</ph><ept id="p27">
            &lt;/defs&gt;</ept><ph id="ph64">
            &lt;clipPath id="SVGID_10_"&gt;
                &lt;use xlink:href="#SVGID_9_"  style="overflow:visible;"/&gt;
            &lt;/clipPath&gt;
            &lt;g class="st44"&gt;</ph><bpt id="p28">
                &lt;defs&gt;</bpt><ph id="ph65">
                    &lt;rect id="SVGID_11_" x="431.6" y="542.4" width="34.1" height="34.1"/&gt;</ph><ept id="p28">
                &lt;/defs&gt;</ept><ph id="ph66">
                &lt;clipPath id="SVGID_12_"&gt;
                    &lt;use xlink:href="#SVGID_11_"  style="overflow:visible;"/&gt;
                &lt;/clipPath&gt;
                &lt;path class="st45" d="M436.5,572.8c-0.9,0-1.6-0.4-2-1c-0.4-0.7-0.4-1.5,0.1-2.4l12.6-22.1c0.4-0.8,1.1-1.2,1.9-1.2
                    c0.8,0,1.5,0.4,1.9,1.2l12.6,22.1c0.4,0.8,0.5,1.7,0.1,2.4c-0.4,0.7-1.1,1.1-2,1.1H436.5z"/&gt;
                &lt;path class="st46" d="M436.5,571.8c-1.1,0-1.5-0.9-1-1.8l12.6-22.1c0.5-0.9,1.4-0.9,2,0l12.6,22.1c0.5,0.9,0.1,1.8-1,1.8H436.5z
                    "/&gt;
                &lt;path class="st45" d="M450.1,559.1c0.3-0.1,0.7-0.3,0.9-0.5l2.5,5c-0.3,0.1-0.6,0.3-0.9,0.5L450.1,559.1z"/&gt;
                &lt;path class="st45" d="M451.5,566h-4.8c0,0.2,0,0.3,0,0.5c0,0.2,0,0.3,0,0.5h4.8c0-0.2,0-0.3,0-0.5
                    C451.4,566.3,451.5,566.2,451.5,566"/&gt;
                &lt;path class="st45" d="M445.6,564.1c-0.3-0.2-0.6-0.4-1-0.5l2.6-4.9c0.3,0.2,0.6,0.4,0.9,0.5L445.6,564.1z"/&gt;
                &lt;path class="st47" d="M449.1,559c-1.5,0-2.7-1.2-2.7-2.7c0-1.5,1.2-2.7,2.7-2.7c1.5,0,2.7,1.2,2.7,2.7
                    C451.8,557.7,450.6,559,449.1,559"/&gt;
                &lt;path class="st46" d="M449.1,553.9c1.3,0,2.3,1,2.3,2.3c0,1.3-1,2.3-2.3,2.3c-1.3,0-2.3-1-2.3-2.3
                    C446.8,554.9,447.8,553.9,449.1,553.9 M449.1,553.1c-1.7,0-3.1,1.4-3.1,3.1c0,1.7,1.4,3.1,3.1,3.1c1.7,0,3.1-1.4,3.1-3.1
                    C452.2,554.5,450.8,553.1,449.1,553.1"/&gt;
                &lt;path class="st45" d="M454.5,569.3c-1.5,0-2.7-1.2-2.7-2.7c0-1.5,1.2-2.7,2.7-2.7s2.7,1.2,2.7,2.7S456,569.3,454.5,569.3"/&gt;
                &lt;path class="st46" d="M454.5,564.2c1.3,0,2.3,1,2.3,2.3c0,1.3-1,2.3-2.3,2.3s-2.3-1-2.3-2.3
                    C452.2,565.3,453.2,564.2,454.5,564.2 M454.5,563.4c-1.7,0-3.1,1.4-3.1,3.1c0,1.7,1.4,3.1,3.1,3.1s3.1-1.4,3.1-3.1
                    C457.6,564.8,456.2,563.4,454.5,563.4"/&gt;
                &lt;path class="st45" d="M443.7,569.3c-1.5,0-2.7-1.2-2.7-2.7c0-1.5,1.2-2.7,2.7-2.7s2.7,1.2,2.7,2.7S445.2,569.3,443.7,569.3"/&gt;
                &lt;path class="st46" d="M443.7,564.2c1.3,0,2.3,1,2.3,2.3c0,1.3-1,2.3-2.3,2.3s-2.3-1-2.3-2.3
                    C441.4,565.3,442.4,564.2,443.7,564.2 M443.7,563.4c-1.7,0-3.1,1.4-3.1,3.1c0,1.7,1.4,3.1,3.1,3.1s3.1-1.4,3.1-3.1
                    C446.9,564.8,445.4,563.4,443.7,563.4"/&gt;
            &lt;/g&gt;
        &lt;/g&gt;
    &lt;/g&gt;
    &lt;g&gt;
        &lt;g&gt;</ph><bpt id="p29">
            &lt;defs&gt;</bpt><ph id="ph67">
                &lt;rect id="SVGID_13_" x="361.7" y="536.2" width="59" height="59"/&gt;</ph><ept id="p29">
            &lt;/defs&gt;</ept><ph id="ph68">
            &lt;clipPath id="SVGID_14_"&gt;
                &lt;use xlink:href="#SVGID_13_"  style="overflow:visible;"/&gt;
            &lt;/clipPath&gt;
            &lt;rect x="367.1" y="548" class="st48" width="48.1" height="35.4"/&gt;
            &lt;g class="st49"&gt;</ph><bpt id="p30">
                &lt;defs&gt;</bpt><ph id="ph69">
                    &lt;rect id="SVGID_15_" x="361.7" y="536.2" width="59" height="59"/&gt;</ph><ept id="p30">
                &lt;/defs&gt;</ept><ph id="ph70">
                &lt;clipPath id="SVGID_16_"&gt;
                    &lt;use xlink:href="#SVGID_15_"  style="overflow:visible;"/&gt;
                &lt;/clipPath&gt;
                &lt;path class="st50" d="M391.9,569.4c-0.3-1.4-0.7-2.1-1.8-2.6c-1.1-0.6-1.6-1-2-1c-0.2,0-0.3,0.1-0.4,0.1
                    c-0.3,0.3-0.5,0.4-0.8,0.6c-0.1,0-0.1,0.1-0.1,0.1c-0.3,0.2-0.6,0.3-0.9,0.4c-0.6,0.2-1.1,0.3-1.8,0.3c-0.6,0-1.2-0.1-1.8-0.3
                    c-0.3-0.1-0.6-0.3-0.9-0.4c-0.4-0.2-0.7-0.4-1-0.7c-0.1-0.1-0.3-0.1-0.4-0.1c-0.4,0-0.8,0.3-2,1c-1.1,0.6-1.4,1.3-1.8,2.6
                    c-0.3,1.2-0.6,4.5-0.7,5.9h17C392.5,573.9,392.2,570.6,391.9,569.4"/&gt;
                &lt;path class="st50" d="M384.1,565.4c-2.3,0-4.2-2.1-4.2-4.6s1.9-4.6,4.2-4.6c2.3,0,4.2,2.1,4.2,4.6S386.4,565.4,384.1,565.4"/&gt;
                &lt;path class="st50" d="M368.9,581.6h44.5v-31.8h-44.5V581.6z M411.6,579.8h-40.8v-28.1h40.8V579.8z"/&gt;
                &lt;rect x="396.2" y="559.8" class="st50" width="11.8" height="1.8"/&gt;
                &lt;rect x="396.2" y="565.3" class="st50" width="11.8" height="1.8"/&gt;
                &lt;rect x="396.2" y="570.7" class="st50" width="11.8" height="1.8"/&gt;
            &lt;/g&gt;
        &lt;/g&gt;
    &lt;/g&gt;
    &lt;g&gt;
        &lt;g&gt;</ph><bpt id="p31">
            &lt;defs&gt;</bpt><ph id="ph71">
                &lt;rect id="SVGID_17_" x="470.1" y="541.1" width="48.9" height="48.9"/&gt;</ph><ept id="p31">
            &lt;/defs&gt;</ept><ph id="ph72">
            &lt;clipPath id="SVGID_18_"&gt;
                &lt;use xlink:href="#SVGID_17_"  style="overflow:visible;"/&gt;
            &lt;/clipPath&gt;
            &lt;g class="st51"&gt;</ph><bpt id="p32">
                &lt;defs&gt;</bpt><ph id="ph73">
                    &lt;rect id="SVGID_19_" x="470.1" y="541.1" width="48.9" height="48.9"/&gt;</ph><ept id="p32">
                &lt;/defs&gt;</ept><ph id="ph74">
                &lt;clipPath id="SVGID_20_"&gt;
                    &lt;use xlink:href="#SVGID_19_"  style="overflow:visible;"/&gt;
                &lt;/clipPath&gt;
                &lt;path class="st52" d="M513.2,585.5l-0.1-1.7c-0.2-2.7-0.8-8.2-1.4-10.8c-0.7-2.8-1.6-4.8-4.2-6.3l-1-0.6
                    c-1.9-1.1-2.7-1.6-3.8-1.6c-0.6,0-1.1,0.1-1.6,0.4l-0.9,0.4l0.7-0.6c2.2-2,3.7-5,3.7-8.4c0-5.9-4.4-10.7-9.8-10.7h0
                    c-5.4,0-9.8,4.8-9.8,10.7c0,3.4,1.4,6.4,3.7,8.4l0.7,0.6l-0.9-0.4c-0.5-0.3-1-0.4-1.6-0.4c-1.1,0-1.9,0.5-3.8,1.6l-1,0.6
                    c-2.7,1.5-3.6,3.5-4.2,6.3c-0.6,2.6-1.2,8.1-1.4,10.8l-0.1,1.7H513.2z"/&gt;
                &lt;path class="st53" d="M510.1,573.3c-0.6-2.8-1.4-4.1-3.5-5.3c-2.1-1.2-3.2-1.9-4-1.9c-0.3,0-0.6,0.1-0.8,0.2
                    c-0.5,0.5-1.1,0.9-1.6,1.2c0,0,0,0,0,0c-0.1,0.1-0.2,0.1-0.3,0.2c-0.6,0.4-1.2,0.6-1.8,0.8c-1.1,0.4-2.2,0.6-3.4,0.6
                    c-1.2,0-2.3-0.2-3.4-0.6c-0.6-0.2-1.2-0.4-1.8-0.8c-0.7-0.4-1.3-0.9-1.9-1.4c-0.2-0.1-0.5-0.2-0.8-0.2c-0.8,0-1.6,0.5-4,1.9
                    c-2.1,1.2-2.8,2.5-3.5,5.3c-0.6,2.4-1.2,7.9-1.4,10.5h33.6C511.3,581.2,510.6,575.7,510.1,573.3"/&gt;
                &lt;path class="st53" d="M494.7,565.4c-4.5,0-8.2-4.1-8.2-9.1s3.7-9.1,8.2-9.1c4.5,0,8.2,4.1,8.2,9.1S499.2,565.4,494.7,565.4"/&gt;
            &lt;/g&gt;
        &lt;/g&gt;
    &lt;/g&gt;
    &lt;g&gt;
        &lt;g&gt;</ph><bpt id="p33">
            &lt;defs&gt;</bpt><ph id="ph75">
                &lt;rect id="SVGID_21_" x="471.3" y="591.4" width="44.9" height="44.9"/&gt;</ph><ept id="p33">
            &lt;/defs&gt;</ept><ph id="ph76">
            &lt;clipPath id="SVGID_22_"&gt;
                &lt;use xlink:href="#SVGID_21_"  style="overflow:visible;"/&gt;
            &lt;/clipPath&gt;
            &lt;g class="st54"&gt;</ph><bpt id="p34">
                &lt;defs&gt;</bpt><ph id="ph77">
                    &lt;rect id="SVGID_23_" x="471.3" y="591.4" width="44.9" height="44.9"/&gt;</ph><ept id="p34">
                &lt;/defs&gt;</ept><ph id="ph78">
                &lt;clipPath id="SVGID_24_"&gt;
                    &lt;use xlink:href="#SVGID_23_"  style="overflow:visible;"/&gt;
                &lt;/clipPath&gt;
                &lt;path class="st55" d="M489.5,621.2l0.1-1.6c0.1-1.8,0.6-5.6,1-7.3c0.5-2.1,1.1-3.5,3.1-4.6l0.7-0.4c1.3-0.8,2-1.1,2.9-1.1
                    c0.5,0,0.9,0.1,1.4,0.3l0.2,0.1l0.2,0.1c0.3,0.3,0.7,0.5,1,0.7c0.3,0.2,0.6,0.3,0.8,0.4l0.1,0c0.6,0.2,1.1,0.3,1.7,0.3
                    c0.6,0,1.2-0.1,1.7-0.3c0.3-0.1,0.6-0.2,0.9-0.4l0,0l0.4-0.3l0.1,0c0.2-0.1,0.3-0.2,0.5-0.4l0.2-0.1l0.2-0.1
                    c0.4-0.2,0.9-0.3,1.4-0.3c0.9,0,1.6,0.4,2.8,1.1c0.2,0.1,0.5,0.3,0.7,0.4c2,1.1,2.7,2.6,3.1,4.6c0.4,1.7,0.8,5.5,1,7.3l0.1,1.6
                    H489.5z"/&gt;
                &lt;path class="st56" d="M512.8,612.6c-0.4-1.8-0.9-2.7-2.3-3.5c-1.4-0.8-2.2-1.3-2.7-1.3c-0.2,0-0.4,0.1-0.6,0.1
                    c-0.3,0.3-0.7,0.6-1.1,0.8c0,0,0,0,0,0c-0.1,0-0.1,0.1-0.2,0.1c-0.4,0.2-0.8,0.4-1.2,0.5c-0.7,0.3-1.5,0.4-2.3,0.4
                    c-0.8,0-1.6-0.1-2.3-0.4c-0.4-0.1-0.8-0.3-1.2-0.5c-0.5-0.3-0.9-0.6-1.3-0.9c-0.2-0.1-0.3-0.1-0.6-0.1c-0.5,0-1.1,0.4-2.7,1.3
                    c-1.4,0.8-1.9,1.7-2.3,3.5c-0.4,1.6-0.8,5.3-0.9,7.1h22.4C513.6,617.9,513.2,614.3,512.8,612.6"/&gt;
                &lt;path class="st55" d="M502.5,609.1c-4,0-7.2-3.5-7.2-7.8c0-4.3,3.2-7.8,7.2-7.8c4,0,7.2,3.5,7.2,7.8
                    C509.7,605.6,506.5,609.1,502.5,609.1"/&gt;
                &lt;path class="st56" d="M502.5,607.4c-3,0-5.5-2.7-5.5-6.1c0-3.4,2.5-6.1,5.5-6.1c3,0,5.5,2.7,5.5,6.1
                    C508,604.7,505.5,607.4,502.5,607.4"/&gt;
                &lt;path class="st55" d="M472.4,626.7l0.1-1.6c0.1-1.8,0.6-5.6,1-7.3c0.5-2.1,1.1-3.5,3.1-4.6l0.6-0.4c1.3-0.8,2-1.2,2.9-1.2
                    c0.5,0,1,0.1,1.4,0.3l0.2,0.1l0.2,0.1c0.3,0.3,0.7,0.5,1,0.7c0.3,0.2,0.6,0.3,0.8,0.4l0.1,0c0.6,0.2,1.1,0.3,1.7,0.3
                    c0.6,0,1.2-0.1,1.7-0.3l0.1,0c0.3-0.1,0.5-0.2,0.8-0.4l0.1-0.1l0.4-0.3h0.1c0.2-0.1,0.3-0.2,0.5-0.4l0.2-0.1l0.2-0.1
                    c0.4-0.2,0.9-0.3,1.4-0.3c0.9,0,1.6,0.4,2.8,1.1c0.2,0.1,0.5,0.3,0.7,0.4c2,1.1,2.7,2.6,3.1,4.6c0.4,1.8,0.8,5.5,1,7.3l0.1,1.6
                    H472.4z"/&gt;
                &lt;path class="st56" d="M495.8,618.1c-0.4-1.8-0.9-2.7-2.3-3.5c-1.4-0.8-2.2-1.3-2.7-1.3c-0.2,0-0.4,0.1-0.6,0.1
                    c-0.3,0.3-0.7,0.6-1.1,0.8c0,0,0,0,0,0c-0.1,0-0.1,0.1-0.2,0.1c-0.4,0.2-0.8,0.4-1.2,0.5c-0.7,0.3-1.5,0.4-2.3,0.4
                    c-0.8,0-1.6-0.1-2.3-0.4c-0.4-0.1-0.8-0.3-1.2-0.5c-0.5-0.3-0.9-0.6-1.3-0.9c-0.2-0.1-0.3-0.1-0.6-0.1c-0.5,0-1.1,0.4-2.7,1.3
                    c-1.4,0.8-1.9,1.7-2.3,3.5c-0.4,1.6-0.8,5.3-0.9,7.1h22.4C496.6,623.4,496.1,619.7,495.8,618.1"/&gt;
                &lt;path class="st55" d="M485.5,614.6c-4,0-7.2-3.5-7.2-7.8c0-4.3,3.2-7.8,7.2-7.8c4,0,7.2,3.5,7.2,7.8
                    C492.6,611.1,489.4,614.6,485.5,614.6"/&gt;
                &lt;path class="st56" d="M485.5,612.9c-3,0-5.5-2.7-5.5-6.1c0-3.4,2.5-6.1,5.5-6.1c3,0,5.5,2.7,5.5,6.1
                    C490.9,610.1,488.5,612.9,485.5,612.9"/&gt;
                &lt;path class="st55" d="M484.5,634.3l0.1-1.6c0.1-1.8,0.6-5.6,1-7.3c0.5-2.1,1.1-3.5,3.1-4.6l0.6-0.4c1.3-0.8,2-1.1,2.9-1.1
                    c0.5,0,1,0.1,1.4,0.3l0.2,0.1l0.2,0.1c0.3,0.3,0.6,0.5,1,0.7c0.3,0.2,0.6,0.3,0.8,0.4l0.1,0c0.6,0.2,1.2,0.3,1.7,0.3
                    c0.6,0,1.2-0.1,1.7-0.3l0.1,0c0.3-0.1,0.5-0.2,0.8-0.4l0.1,0l0.4-0.3h0.1c0.2-0.1,0.3-0.2,0.5-0.4l0.2-0.1l0.2-0.1
                    c0.4-0.2,0.9-0.3,1.4-0.3c0.9,0,1.6,0.4,2.8,1.1c0.2,0.1,0.5,0.3,0.7,0.4c2,1.1,2.7,2.6,3.1,4.6c0.4,1.7,0.8,5.5,1,7.3l0.1,1.6
                    H484.5z"/&gt;
                &lt;path class="st56" d="M507.8,625.8c-0.4-1.8-0.9-2.7-2.3-3.5c-1.4-0.8-2.2-1.3-2.7-1.3c-0.2,0-0.4,0.1-0.6,0.1
                    c-0.3,0.3-0.7,0.6-1.1,0.8c0,0,0,0,0,0c-0.1,0-0.1,0.1-0.2,0.1c-0.4,0.2-0.8,0.4-1.2,0.5c-0.7,0.3-1.5,0.4-2.3,0.4
                    c-0.8,0-1.6-0.1-2.3-0.4c-0.4-0.1-0.8-0.3-1.2-0.5c-0.5-0.3-0.9-0.6-1.3-0.9c-0.2-0.1-0.3-0.1-0.6-0.1c-0.5,0-1.1,0.4-2.7,1.3
                    c-1.4,0.8-1.9,1.7-2.3,3.5c-0.4,1.6-0.8,5.3-0.9,7.1h22.4C508.6,631.1,508.2,627.4,507.8,625.8"/&gt;
                &lt;path class="st55" d="M497.5,622.2c-4,0-7.2-3.5-7.2-7.8c0-4.3,3.2-7.8,7.2-7.8c4,0,7.2,3.5,7.2,7.8
                    C504.7,618.7,501.5,622.2,497.5,622.2"/&gt;
                &lt;path class="st56" d="M497.5,620.5c-3,0-5.5-2.7-5.5-6.1c0-3.4,2.5-6.1,5.5-6.1c3,0,5.5,2.7,5.5,6.1
                    C503,617.8,500.5,620.5,497.5,620.5"/&gt;
            &lt;/g&gt;
        &lt;/g&gt;
    &lt;/g&gt;</ph><bpt id="p35">
    &lt;g&gt;</bpt><bpt id="p36">
        &lt;g&gt;</bpt><ph id="ph79">
            &lt;path class="st1" d="M197.8,600.9c-2.3,0-4.3-1.9-4.3-4.2V567c0-2.3,2-4.2,4.3-4.2h40c2.3,0,4.2,1.9,4.2,4.2v29.7
                c0,2.3-1.9,4.2-4.2,4.2H197.8z"/&gt;</ph><ept id="p36">
        &lt;/g&gt;</ept><ph id="ph80">
        &lt;path class="st10" d="M237.9,564.5h-40c-1.4,0-2.6,1.1-2.6,2.5v29.7c0,1.4,1.2,2.5,2.6,2.5h40.1c1.4,0,2.5-1.1,2.5-2.5V567
            C240.4,565.6,239.3,564.5,237.9,564.5z M238.6,596.7c0,0.4-0.3,0.7-0.7,0.7h-40.1c-0.4,0-0.8-0.4-0.8-0.7V567
            c0-0.4,0.4-0.8,0.9-0.8h40c0.4,0,0.7,0.3,0.7,0.7V596.7z"/&gt;</ph><bpt id="p37">
        &lt;g&gt;</bpt><ph id="ph81">
            &lt;path class="st10" d="M230.8,587.1v-5.2H230h-0.9h-10.4v-4.3h4.3v-7.8h-10.4v7.8h4.3v4.3h-10.4h-0.9h-0.9v5.2h-3.5v6.9h8.7v-6.9
                h-3.5v-3.5h10.4v3.5h-3.5v6.9h8.7v-6.9h-3.5v-3.5h10.4v3.5h-3.5v6.9h8.7v-6.9H230.8z"/&gt;</ph><ept id="p37">
        &lt;/g&gt;</ept><ept id="p35">
    &lt;/g&gt;</ept><ph id="ph82">
    &lt;g&gt;
        &lt;g&gt;</ph><bpt id="p38">
            &lt;defs&gt;</bpt><ph id="ph83">
                &lt;rect id="SVGID_25_" x="333.6" y="561.7" width="52.2" height="52.2"/&gt;</ph><ept id="p38">
            &lt;/defs&gt;</ept><ph id="ph84">
            &lt;clipPath id="SVGID_26_"&gt;
                &lt;use xlink:href="#SVGID_25_"  style="overflow:visible;"/&gt;
            &lt;/clipPath&gt;
            &lt;g class="st57"&gt;</ph><bpt id="p39">
                &lt;defs&gt;</bpt><ph id="ph85">
                    &lt;rect id="SVGID_27_" x="333.6" y="561.7" width="52.2" height="52.2"/&gt;</ph><ept id="p39">
                &lt;/defs&gt;</ept><ph id="ph86">
                &lt;clipPath id="SVGID_28_"&gt;
                    &lt;use xlink:href="#SVGID_27_"  style="overflow:visible;"/&gt;
                &lt;/clipPath&gt;
                &lt;path class="st58" d="M339.2,608.3v-36.9c0-1.8,1.4-3.2,3.2-3.2h35.3c1.8,0,3.2,1.4,3.2,3.2v36.9H339.2z"/&gt;
                &lt;path class="st59" d="M379.4,606.7v-34.9c0-1.1-0.9-2-2-2h-34.5c-1.1,0-2,0.9-2,2v34.9H379.4z M377.8,605.1h-35.3v-28.9h35.3
                    V605.1z"/&gt;
                &lt;rect x="351.2" y="578.6" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="357.7" y="578.6" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="364.1" y="578.6" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="370.5" y="578.6" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="344.8" y="585" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="351.2" y="585" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="357.7" y="585" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="364.1" y="585" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="370.5" y="585" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="344.8" y="591.4" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="351.2" y="591.4" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="357.7" y="591.4" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="364.1" y="591.4" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="370.5" y="591.4" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="344.8" y="597.8" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="351.2" y="597.8" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="357.7" y="597.8" class="st59" width="4.8" height="4.8"/&gt;
                &lt;rect x="364.1" y="597.8" class="st59" width="4.8" height="4.8"/&gt;
            &lt;/g&gt;
        &lt;/g&gt;
    &lt;/g&gt;
    &lt;g&gt;
        &lt;g&gt;</ph><bpt id="p40">
            &lt;defs&gt;</bpt><ph id="ph87">
                &lt;rect id="SVGID_29_" x="295" y="536.7" width="60.1" height="60.1"/&gt;</ph><ept id="p40">
            &lt;/defs&gt;</ept><ph id="ph88">
            &lt;clipPath id="SVGID_30_"&gt;
                &lt;use xlink:href="#SVGID_29_"  style="overflow:visible;"/&gt;
            &lt;/clipPath&gt;
            &lt;rect x="302.4" y="548.8" class="st60" width="45.3" height="36.1"/&gt;
            &lt;path class="st61" d="M304.2,583h41.6v-32.4h-41.6V583z M344,581.1h-37.9v-28.7H344V581.1z"/&gt;
            &lt;rect x="334.7" y="556.2" class="st61" width="5.5" height="6.5"/&gt;
            &lt;rect x="312.5" y="567.2" class="st61" width="16.6" height="1.8"/&gt;
            &lt;rect x="312.5" y="572.8" class="st61" width="21.3" height="1.8"/&gt;
        &lt;/g&gt;
    &lt;/g&gt;
    &lt;g&gt;
        &lt;g&gt;</ph><bpt id="p41">
            &lt;defs&gt;</bpt><ph id="ph89">
                &lt;rect id="SVGID_31_" x="157.3" y="542.1" width="55.2" height="55.2"/&gt;</ph><ept id="p41">
            &lt;/defs&gt;</ept><ph id="ph90">
            &lt;clipPath id="SVGID_32_"&gt;
                &lt;use xlink:href="#SVGID_31_"  style="overflow:visible;"/&gt;
            &lt;/clipPath&gt;
            &lt;polygon class="st62" points="166.6,592.2 166.6,548 191.1,548 203.1,560.1 203.1,592.2           "/&gt;
            &lt;path class="st63" d="M201.4,590.5v-29.7l-11-11h-22.1v40.8H201.4z M170,588.8v-37.4h18.7v11h11v26.3H170z"/&gt;
            &lt;rect x="175.9" y="570.1" class="st63" width="17.8" height="1.7"/&gt;
            &lt;rect x="175.9" y="575.2" class="st63" width="17.8" height="1.7"/&gt;
            &lt;rect x="175.9" y="580.3" class="st63" width="17.8" height="1.7"/&gt;
        &lt;/g&gt;
    &lt;/g&gt;</ph><bpt id="p42">
    &lt;g&gt;</bpt><ph id="ph91">
        &lt;path class="st1" d="M370.5,631v-40.4h36V631H370.5z"/&gt;</ph><bpt id="p43">
        &lt;g&gt;</bpt><ph id="ph92">
            &lt;path class="st10" d="M396.4,612.3c-0.3-1.5-0.7-2.2-1.8-2.8c-1.1-0.6-1.7-1-2.1-1c-0.2,0-0.3,0-0.4,0.1
                c-0.3,0.2-0.6,0.5-0.9,0.6l0,0c-0.1,0-0.1,0.1-0.2,0.1c-0.3,0.2-0.6,0.3-0.9,0.4c-0.6,0.2-1.2,0.3-1.8,0.3
                c-0.6,0-1.2-0.1-1.8-0.3c-0.3-0.1-0.6-0.2-0.9-0.4c-0.4-0.2-0.7-0.5-1-0.7c-0.1-0.1-0.3-0.1-0.4-0.1c-0.4,0-0.8,0.3-2.1,1
                c-1.1,0.6-1.5,1.3-1.8,2.8c-0.3,1.3-0.6,4.9-0.7,6.3h17.7C397,617.2,396.7,613.6,396.4,612.3z"/&gt;
            &lt;path class="st10" d="M388.3,608.2c-2.4,0-4.3-2.1-4.3-4.8c0-2.6,1.9-4.8,4.3-4.8c2.4,0,4.3,2.1,4.3,4.8
                C392.6,606,390.7,608.2,388.3,608.2z"/&gt;</ph><ept id="p43">
        &lt;/g&gt;</ept><ph id="ph93">
        &lt;path class="st10" d="M372.5,592.6v35h32v-35H372.5z M402.5,622.6h-28v-28h28V622.6z"/&gt;</ph><ept id="p42">
    &lt;/g&gt;</ept><ph id="ph94">
    &lt;g&gt;
        &lt;rect x="305.2" y="602.7" class="st64" width="33.3" height="31.9"/&gt;
        &lt;circle class="st64" cx="335.9" cy="606.7" r="9.3"/&gt;
        &lt;text transform="matrix(1 0 0 1 331.8331 611.0155)" class="st10 st65 st66"&gt;</ph>1<ph id="ph95">&lt;/text&gt;
    &lt;/g&gt;
    &lt;g&gt;
        &lt;rect x="167.7" y="602.7" class="st64" width="42.2" height="28.2"/&gt;
        &lt;circle class="st67" cx="188.8" cy="616.7" r="9.3"/&gt;
        &lt;g&gt;
            &lt;g&gt;</ph><bpt id="p44">
                &lt;defs&gt;</bpt><ph id="ph96">
                    &lt;rect id="SVGID_33_" x="180.9" y="608.1" width="17.8" height="17.8"/&gt;</ph><ept id="p44">
                &lt;/defs&gt;</ept><ph id="ph97">
                &lt;clipPath id="SVGID_34_"&gt;
                    &lt;use xlink:href="#SVGID_33_"  style="overflow:visible;"/&gt;
                &lt;/clipPath&gt;
                &lt;g class="st68"&gt;</ph><bpt id="p45">
                    &lt;defs&gt;</bpt><ph id="ph98">
                        &lt;rect id="SVGID_35_" x="180.9" y="608.1" width="17.8" height="17.8"/&gt;</ph><ept id="p45">
                    &lt;/defs&gt;</ept><ph id="ph99">
                    &lt;clipPath id="SVGID_36_"&gt;
                        &lt;use xlink:href="#SVGID_35_"  style="overflow:visible;"/&gt;
                    &lt;/clipPath&gt;
                    &lt;path class="st69" d="M184.2,612.2c0-0.4,0.2-0.7,0.5-0.9c0.3-0.2,0.7-0.2,1,0l8.9,4.8c0.4,0.2,0.6,0.5,0.6,0.9
                        c0,0.4-0.2,0.7-0.6,0.9l-8.9,4.8c-0.3,0.2-0.7,0.2-1,0c-0.3-0.2-0.5-0.5-0.5-0.9V612.2z"/&gt;
                    &lt;path class="st70" d="M185.4,622.3l8.9-4.8c0.4-0.2,0.4-0.6,0-0.8l-8.9-4.8c-0.4-0.2-0.7,0-0.7,0.4v9.7
                        C184.7,622.3,185,622.5,185.4,622.3"/&gt;
                &lt;/g&gt;
            &lt;/g&gt;
        &lt;/g&gt;
    &lt;/g&gt;
    &lt;circle class="st10" cx="354" cy="492.6" r="4.1"/&gt;
    &lt;circle class="st10" cx="178.6" cy="259.2" r="4.1"/&gt;
    &lt;circle class="st10" cx="295.7" cy="259.2" r="4.1"/&gt;
    &lt;circle class="st10" cx="413" cy="259.2" r="4.1"/&gt;
    &lt;circle class="st7" cx="529.6" cy="218.7" r="4.1"/&gt;
    &lt;text transform="matrix(1 0 0 1 312.4307 340.2214)" class="st1 st2 st3"&gt;</ph>v2.0 app model<ph id="ph100">&lt;/text&gt;</ph><bpt id="p46">
    &lt;g&gt;</bpt><ph id="ph101">
        &lt;polygon class="st71" points="173,129.6 159.1,118.7 172.3,101.9 186.4,118.7         "/&gt;
        &lt;circle class="st21" cx="172.7" cy="110.3" r="2"/&gt;
        &lt;circle class="st21" cx="166.6" cy="117.7" r="2"/&gt;
        &lt;circle class="st21" cx="178.5" cy="117.7" r="2"/&gt;
        &lt;circle class="st21" cx="172.7" cy="122.8" r="2"/&gt;
        &lt;line class="st72" x1="172.7" y1="110.3" x2="172.7" y2="122.8"/&gt;
        &lt;line class="st72" x1="172.7" y1="110.3" x2="166.6" y2="117.7"/&gt;
        &lt;line class="st72" x1="172.7" y1="110.3" x2="178.5" y2="117.7"/&gt;
        &lt;line class="st72" x1="166.6" y1="117.7" x2="172.7" y2="123.5"/&gt;
        &lt;line class="st72" x1="172.7" y1="123.5" x2="178.5" y2="117.7"/&gt;</ph><ept id="p46">
    &lt;/g&gt;</ept><bpt id="p47">
    &lt;g&gt;</bpt><ph id="ph102">
        &lt;polygon class="st71" points="145.3,394.4 131.4,383.5 144.7,366.7 158.7,383.5       "/&gt;
        &lt;circle class="st21" cx="145" cy="375.1" r="2"/&gt;
        &lt;circle class="st21" cx="138.9" cy="382.5" r="2"/&gt;
        &lt;circle class="st21" cx="150.8" cy="382.5" r="2"/&gt;
        &lt;circle class="st21" cx="145" cy="387.6" r="2"/&gt;
        &lt;line class="st72" x1="145" y1="375.1" x2="145" y2="387.6"/&gt;
        &lt;line class="st72" x1="145" y1="375.1" x2="138.9" y2="382.5"/&gt;
        &lt;line class="st72" x1="145" y1="375.1" x2="150.8" y2="382.5"/&gt;
        &lt;line class="st72" x1="138.9" y1="382.5" x2="145" y2="388.4"/&gt;
        &lt;line class="st72" x1="145" y1="388.4" x2="150.8" y2="382.5"/&gt;</ph><ept id="p47">
    &lt;/g&gt;</ept><ph id="ph103">
    &lt;g&gt;
        &lt;g&gt;</ph><bpt id="p48">
            &lt;defs&gt;</bpt><ph id="ph104">
                &lt;rect id="SVGID_37_" x="419.4" y="17.1" width="26.5" height="26.5"/&gt;</ph><ept id="p48">
            &lt;/defs&gt;</ept><ph id="ph105">
            &lt;clipPath id="SVGID_38_"&gt;
                &lt;use xlink:href="#SVGID_37_"  style="overflow:visible;"/&gt;
            &lt;/clipPath&gt;
            &lt;polygon class="st73" points="430.8,30.1 430.8,22.1 422.7,23.2 422.7,30.1           "/&gt;
            &lt;polygon class="st73" points="431.2,30.1 442.7,30.1 442.7,20.3 431.2,22             "/&gt;
            &lt;polygon class="st73" points="431.2,30.6 431.2,38.7 442.7,40.4 442.7,30.6           "/&gt;
            &lt;polygon class="st73" points="430.8,30.6 422.7,30.6 422.7,37.5 430.8,38.6           "/&gt;
        &lt;/g&gt;
    &lt;/g&gt;
    &lt;text transform="matrix(1 0 0 1 516.4418 38.8259)" class="st1 st2 st29"&gt;</ph>...<ph id="ph106">&lt;/text&gt;
    &lt;rect x="451" y="23.3" class="st74" width="38.5" height="16"/&gt;
    &lt;text transform="matrix(1 0 0 1 451.0208 38.3713)" class="st1 st75 st29"&gt;</ph>iOS<ph id="ph107">&lt;/text&gt;</ph><bpt id="p49">
    &lt;g&gt;</bpt><ph id="ph108">
        &lt;path class="st21" d="M496.9,32.9c0,0.6-0.5,1.2-1.1,1.2c-0.6,0-1.1-0.5-1.1-1.2V27c0-0.6,0.5-1.2,1.1-1.2c0.6,0,1.1,0.5,1.1,1.2
            V32.9L496.9,32.9L496.9,32.9z"/&gt;
        &lt;path class="st21" d="M512.9,32.9c0,0.6-0.5,1.2-1.1,1.2s-1.1-0.5-1.1-1.2V27c0-0.6,0.5-1.2,1.1-1.2c0.6,0,1.1,0.5,1.1,1.2V32.9
            L512.9,32.9L512.9,32.9z"/&gt;
        &lt;path class="st21" d="M498,25.8v8.7c0,1,0.7,1.8,1.6,1.8h0.7v3c0,0.6,0.5,1.2,1.1,1.2c0.6,0,1.1-0.5,1.1-1.2v-3h2.3v3
            c0,0.6,0.5,1.2,1.1,1.2s1.1-0.5,1.1-1.2v-3h1.1c0.9,0,1.3-0.8,1.3-1.8v-8.7H498L498,25.8L498,25.8z"/&gt;
        &lt;path class="st21" d="M510,24.5c0,0,0-0.1,0-0.2c0-3.1-2.8-5.6-6.2-5.6c-3.4,0-6.2,2.5-6.2,5.6c0,0.1,0,0.2,0,0.2H510L510,24.5
            L510,24.5z"/&gt;
        &lt;path class="st21" d="M501.6,20.8c0.1,0.1,0,0.3-0.1,0.4c-0.1,0.1-0.3,0-0.4-0.1l-1.7-3.1c-0.1-0.1,0-0.3,0.1-0.4
            c0.1-0.1,0.3,0,0.4,0.1L501.6,20.8L501.6,20.8L501.6,20.8z"/&gt;
        &lt;path class="st21" d="M505.9,20.8c-0.1,0.1,0,0.3,0.1,0.4c0.1,0.1,0.3,0,0.4-0.1l1.7-3.1c0.1-0.1,0-0.3-0.1-0.4
            c-0.1-0.1-0.3,0-0.4,0.1L505.9,20.8L505.9,20.8L505.9,20.8z"/&gt;
        &lt;path class="st76" d="M501.5,21.7c0,0.3-0.3,0.6-0.6,0.6c-0.3,0-0.6-0.3-0.6-0.6s0.3-0.6,0.6-0.6
            C501.3,21.1,501.5,21.4,501.5,21.7L501.5,21.7z"/&gt;
        &lt;path class="st76" d="M507.2,21.7c0-0.3-0.3-0.6-0.6-0.6c-0.3,0-0.6,0.3-0.6,0.6s0.3,0.6,0.6,0.6C507,22.3,507.2,22,507.2,21.7
            L507.2,21.7z"/&gt;</ph><ept id="p49">
    &lt;/g&gt;</ept><bpt id="p50">
    &lt;g&gt;</bpt><bpt id="p51">
        &lt;g&gt;</bpt><ph id="ph109">
            &lt;path class="st1" d="M239.4,614.8c0-1.2-1-2.2-2.2-2.2h-0.3v-0.2c0-1.2-1-2.2-2.2-2.2H225v-3.8h-1.4l-22.4,3.9v31.4l22.4,3.9h1.4
                v-3.8h9.6c1.1,0,2-0.8,2.2-1.9h0.3c1.2,0,2.2-1,2.2-2.2v-7c0-0.2,0-0.5-0.1-0.7c0.1-0.2,0.1-0.4,0.1-0.7V623c0-0.2,0-0.5-0.1-0.7
                c0.1-0.2,0.1-0.5,0.1-0.7V614.8z"/&gt;
            &lt;path class="st10" d="M238.1,614.8c0-0.5-0.4-1-1-1h-1.5v-1.5c0-0.5-0.4-1-1-1h-10.8v-3.8l-21.2,3.7v29.3l21.2,3.7v-3.8v0h10.8
                c0.5,0,1-0.4,1-1v-0.9h1.5c0.5,0,1-0.4,1-1v-7c0-0.3-0.1-0.5-0.3-0.7c0.2-0.2,0.3-0.4,0.3-0.7V623c0-0.3-0.1-0.5-0.3-0.7
                c0.2-0.2,0.3-0.4,0.3-0.7V614.8z M236.9,637.2c0,0.2-0.1,0.3-0.3,0.3h-0.9v-6.9h0.9c0.2,0,0.3,0.1,0.3,0.3V637.2z M236.9,629.1
                c0,0.2-0.1,0.3-0.3,0.3h-0.9v-6.9h0.9c0.2,0,0.3,0.1,0.3,0.3V629.1z M236.9,621.1c0,0.1-0.1,0.3-0.3,0.3h-2.2v18.1h-10.6v-26.8
                h10.6v2.5h2.2c0.1,0,0.3,0.1,0.3,0.3V621.1z"/&gt;</ph><ept id="p51">
        &lt;/g&gt;</ept><bpt id="p52">
        &lt;g&gt;</bpt><ph id="ph110">
            &lt;rect x="221.9" y="615.2" class="st10" width="10" height="1.7"/&gt;
            &lt;rect x="221.9" y="619" class="st10" width="10" height="1.7"/&gt;
            &lt;rect x="221.9" y="622.7" class="st10" width="10" height="1.7"/&gt;
            &lt;rect x="221.9" y="626.5" class="st10" width="10" height="1.7"/&gt;
            &lt;rect x="221.9" y="630.2" class="st10" width="10" height="1.7"/&gt;
            &lt;rect x="221.9" y="634" class="st10" width="10" height="1.7"/&gt;</ph><ept id="p52">
        &lt;/g&gt;</ept><ept id="p50">
    &lt;/g&gt;</ept><ph id="ph111">
&lt;/g&gt;
&lt;/svg&gt;





&lt;!--
&lt;a name="PlannerAPIIntro"&gt; &lt;/a&gt;

###Planner API (preview)

The Planner API enables you to create light-weight project management solutions that create and manage projects, assign project tasks to users, and track the progress and completion of those tasks.

Additional value-prop or technical information about the Planner API, so that developers can decide if they want to know even more. Maybe a paragraph or two at most.

For more information, see [link to Planner API topic].
--&gt;</ph>

###v2.0 アプリ認証モデル プレビューのサポート

v2.0 アプリ認証モデル プレビューを使用すると、会社や学校の ID (Azure AD) と、個人 (Microsoft アカウント) ID の両方を受け入れるアプリを作成できます。 

以前は、Microsoft アカウントと Azure Active Directory の両方をサポートするアプリの開発者は、完全に独立した 2 つのシステムを統合する必要がありました。現在は、v2.0 アプリケーション モデルを使用したアプリを作成することで、両方の種類のアカウントでユーザーがサインインできるようになりました。 

現時点で v2.0 アプリ認証モデル プレビューをアプリで使用してアクセスできる API は次のとおりです。

- Outlook のメール 
- Outlook の連絡先 
- Outlook の予定表

詳細については、「[v2.0 アプリ モード プレビューを使用して Office 365 および Outlook.com API を認証する](..\howto\authenticate-Office-365-APIs-using-v2.md)」を参照してください。

<a name="OfficeGraphIntro"> </a>

###Office Graph 
Office Graph には、Office 365 エンティティおよびエンティティ間のリレーションシップ_について_のデータを、グラフ インデックス内のノードおよびエッジとして格納します。 企業エンティティの例は、**person** と **document** であり、関係の例は、**shared** と **modified** です。 

Office Graph では、すべての Office 365 サービスから受信するデータを接続および完成させるために、高度な解析と機械学習テクニックを使用しています。 

現在プレビュー中の 2 つの関係は、 **TrendingAround** と **WorkingWith** です。これらの関係のクエリ方法の詳細については、「[Office Graph を使った開発](https://msdn.microsoft.com/office/office365/howto/develop-office-graph)」を参照してください。 

<!-- no longer in preview

###Outlook Notifications REST API (preview)
The Notifications API provides change notifications for entities like mail, contacts, events, and groups in Office 365. 
These push notifications use the concept of webhooks as a callback mechanism to provide the change notifications.
During the Preview period, you can access this API at the REST endpoint https://outlook.office365.com/api/beta/{user_context}.

For more information see [Outlook Notifications REST API reference (preview)](../api/notify-rest-operations.md).   

###Outlook User Photo REST API (preview)

The User Photo API allows you to get information about and download the photo of any user in your organization. 
During the Preview period, you can access this API at the REST endpoint https://outlook.office365.com/api/beta/{user_context}.

For more information, see [Outlook User Photo REST API reference (preview)](../api/photo-rest-operations.md).

-->

###ソリューションで複数のプレビュー機能を使用する

プレビュー機能は現在開発中のため、指定されたプレビュー機能がまだ完全に他のプレビュー機能と統合されていなかったり、他のプレビュー機能をサポートしていない可能性があります。他のプレビュー機能との相互運用やサポートについては、特定のプレビュー機能のドキュメントを参照してください。

##Office 365 開発プラットフォームのその他のプレビュー機能

前述の Office 365 のプレビュー API に加え、大規模な Office 365 開発プラットフォーム向けに、次のプレビュー機能が用意されています。

###サービス通信 API (プレビュー)

Office 365 サービス通信 API の新しいバージョンでは、REST API を使用して、Office 365 のテナント管理者およびパートナーにサービスの正常性の情報を提供しています。 操作のリファレンスについては、「[Office 365 サービス通信 API リファレンス (プレビュー)](https://msdn.microsoft.com/library/office/dn707386.aspx)」を参照してください。

###管理アクティビティ API (プレビュー)

Office 365 管理アクティビティ API は、Office 365 および Azure Active Directory 動作状況ログからの、ユーザー、管理者、システム、およびポリシー アクションとイベントについての情報を、REST API を使用して Office 365テナントの管理者とパートナーに提供します。 操作のリファレンスについては、「[Office 365 管理アクティビティ API リファレンス (プレビュー)](https://msdn.microsoft.com/library/office/mt227394.aspx)」を参照してください。

