---
title: "FRS"
date: 2023-06-19T20:48:23-05:00
---

# FRS Channel Lookup

{{<rawhtml>}}
<script type="text/javascript">
    function clearFields() {
        console.log("clearing fields");
        document.getElementById("channel").value = null;
        document.getElementById("pcode").value = null;
        document.getElementById("frequency").innerHTML = "";
        document.getElementById("mode").innerHTML = "";
        document.getElementById("tone").innerHTML = "";

    }
    const freqs = [462.5625, 462.5875, 462.6125, 462.6375, 462.6625, 462.6875, 462.7125, 467.5625, 467.5875, 467.6125, 467.6375, 467.6625, 467.6875, 467.7125, 462.5500, 462.5750, 462.6000, 462.6250, 462.6500, 462.6750, 462.7000, 462.7250];
    const codes = ["67.0 Hz", "71.9 Hz", "74.4 Hz", "77.0 Hz", "79.7 Hz", "82.5 Hz", "85.4 Hz", "88.5 Hz", "91.5 Hz", "94.8 Hz", "97.4 Hz", "100.0 Hz", "103.5 Hz", "107.2 Hz", "110.9 Hz", "114.8 Hz", "118.8 Hz", "123.0 Hz", "127.3 Hz", "131.8 Hz", "136.5 Hz", "141.3 Hz", "146.2 Hz", "151.4 Hz", "156.7 Hz", "162.2 Hz", "167.9 Hz", "173.8 Hz", "179.9 Hz", "186.2 Hz", "192.8 Hz", "203.5 Hz", "210.7 Hz", "218.1 Hz", "225.7 Hz", "233.6 Hz", "241.8 Hz", "250.3 Hz", "D023N", "D025N", "D026N", "D031N", "D032N", "D043N", "D047N", "D051N", "D054N", "D065N", "D071N", "D072N", "D073N", "D074N", "D114N", "D115N", "D116N", "D125N", "D131N", "D132N", "D134N", "D143N", "D152N", "D155N", "D156N", "D162N", "D165N", "D172N", "D174N", "D205N", "D223N", "D226N", "D243N", "D244N", "D245N", "D251N", "D261N", "D263N", "D265N", "D271N", "D306N", "D311N", "D315N", "D331N", "D343N", "D346N", "D351N", "D364N", "D365N", "D371N", "D411N", "D412N", "D412N", "D423N", "D431N", "D432N", "D445N", "D464N", "D465N", "D466N", "D503N"];
    function frsLookup() {
        // console.log("data changed");
        let channel = document.getElementById("channel").value;
        let pcode = document.getElementById("pcode").value;
        let mode;
        let freq;
        console.log("channel:", channel, "pcode:", pcode);
        if (pcode > 99) {
            console.log("invalid pcode");
        }
        else if (pcode > 38) {
            mode = "T-DCS";
            pcode = pcode - 1;
            tone = codes[pcode];
        } else {
            mode = "T-CTSS";
            pcode = pcode - 1;
            tone = codes[pcode];
        }
        document.getElementById("mode").innerHTML = mode;
        document.getElementById("tone").innerHTML = tone;

        if (channel > 22) {
            console.log("invalid channel");
        } else {
            channel = channel - 1
            freq = freqs[channel]
            console.log(channel + 1, freq)
        }
        document.getElementById("frequency").innerHTML = freq;

    }
</script>
<div id="lookup"> 
    <label for="channel">Channel:</label>
    <input id="channel" type="number" onkeyup="frsLookup()">
    <label for="ctpcodecss">PCode:</label>
    <input id="pcode" type="number"  onkeyup="frsLookup()">
    <button id="clear" onClick="clearFields()">Clear</button>
    <br>
    <br>
    Frequency: <span id="frequency">462.5625</span> <span id="mode">T-CTSS</span> <span id="tone">67.0 Hz</span>
</div>
{{</rawhtml>}}



## Common channel mappings
Funkprofi T388
FRS Channel | Frequency
---:        |---:
01	        | 462.5625
02	        | 462.5875
03	        | 462.6125
04	        | 462.6375
05	        | 462.6625
06	        | 462.6875
07	        | 462.7125
08	        | 467.5625
09	        | 467.5875
10	        | 467.6125
11	        | 467.6375
12	        | 467.6625
13	        | 467.6875
14	        | 467.7125
15	        | 462.5500
16	        | 462.5750
17	        | 462.6000
18	        | 462.6250
19	        | 462.6500
20	        | 462.6750
21	        | 462.7000
22	        | 462.7250  


CTCSS Option | T-CTSS (uv-5r)
---:         |---
01           |  67.0 Hz
02           |  71.9 Hz
03           |  74.4 Hz
04           |  77.0 Hz
05           |  79.7 Hz
06           |  82.5 Hz
07           |  85.4 Hz
08           |  88.5 Hz
09           |  91.5 Hz
10           |  94.8 Hz
11           |  97.4 Hz
12           | 100.0 Hz
13           | 103.5 Hz
14           | 107.2 Hz
15           | 110.9 Hz
16           | 114.8 Hz
17           | 118.8 Hz
18           | 123.0 Hz
19           | 127.3 Hz
20           | 131.8 Hz
21           | 136.5 Hz
22           | 141.3 Hz
23           | 146.2 Hz
24           | 151.4 Hz
25           | 156.7 Hz
26           | 162.2 Hz
27           | 167.9 Hz
28           | 173.8 Hz
29           | 179.9 Hz
30           | 186.2 Hz
31           | 192.8 Hz
32           | 203.5 Hz
33           | 210.7 Hz
34           | 218.1 Hz
35           | 225.7 Hz
36           | 233.6 Hz
37           | 241.8 Hz
38           | 250.3 Hz

CTCSS Option | T-DCS (uv-5r)
---:         |---
39           | D023N
40           | D025N
41           | D026N
42           | D031N
43           | D032N
44           | D043N
45           | D047N
46           | D051N
47           | D054N
48           | D065N
49           | D071N
50           | D072N
51           | D073N
52           | D074N
53           | D114N
54           | D115N
55           | D116N
56           | D125N
57           | D131N
58           | D132N
59           | D134N
60           | D143N
61           | D152N
62           | D155N
63           | D156N
64           | D162N
65           | D165N
66           | D172N
67           | D174N
68           | D205N
69           | D223N
70           | D226N
71           | D243N
72           | D244N
73           | D245N
74           | D251N
75           | D261N
76           | D263N
77           | D265N
78           | D271N
79           | D306N
80           | D311N
81           | D315N
82           | D331N
83           | D343N
84           | D346N
85           | D351N
86           | D364N
87           | D365N
88           | D371N
89           | D411N
90           | D412N
91           | D412N
92           | D423N
93           | D431N
94           | D432N
95           | D445N
96           | D464N
97           | D465N
98           | D466N
99           | D503N
end of       | funkprofi
100          | D506N
101          | D516N
102          | D532N
103          | D546N
104          | D565N
105          | D606N
106          | D612N
107          | D624N
108          | D627N
109          | D631N
110          | D632N
111          | D654N
112          | D662N
113          | D664N
114          | D703N
115          | D712N
116          | D723N
117          | D731N
118          | D732N
119          | D734N
120          | D743N
121          | D754N
