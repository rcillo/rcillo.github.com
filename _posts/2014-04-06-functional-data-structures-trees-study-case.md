---
layout: post
title: "Functional data structures: trees study case"
description: ""
category:
tags: []
---
{% include JB/setup %}

Carefully crafted data structures are at the heart of most software we rely on: databases, web frameworks, operational systems, games and so forth. Thats because they are a great way to hide the complexity of how to store and retrieve data, providing developers with tools to write simple yet fast and memory efficient software.

Among a myriad of data structures I guess we can agree that trees are ubiquitous. So lets take a look at some peculiarities that arise from funcional tree implementations.

Providing yet another in depth comparison between a reasonable number of tree implementations is beyond the scope of this article.

## Data structures in the functional land

Since imperative languages had been the status quo for the last decades, a huge effort has gone into providing algorithms and data structure that performs efficiently when implemented by those languages.

Back in college time, most students learned algorithms and data structures in languages like C or Java. Not everyone was comfortable with all those pointers. Me included. My personal experience is that concept was far easier to grasp than implementations.

<figure>
  <img width="100%" src="/images/trees/old-not-so-scary-code.png" alt="Some old tree C code. A good one">
  <figcaption>
    Some old tree C code. A not so scary one.
  </figcaption>
</figure>

<figure>
  <img width="100%" src="/images/trees/old-scary-code.png" alt="Some old tree C code. An ugly one.">
  <figcaption>
    Thats what I'm talking about. I would not show this code in an interview.
  </figcaption>
</figure>

But the history has changed after I started reading Chris Okazaki book <sup><a href="#fn-item1" id="fn-return1">1</a></sup> on purely functional data structures. Funcional languages like Haskell and Elixir allow for clearer implementations. In fact, I’m so excited about it, that I wrote this essay.

The old ideas are still the same, but in functional languages we have to work with immutability, a core feature that may looks like limitating at first sight, but offers quite a lot of benefits, as we will see.

## Persistence and sharing: immutability’s twins

Suppose we need to update a value in an array, we just go straight ahead and <pre style="text-align:center;">array[i] = value</pre> update the array at the index position (usually `i`, `j`, `k` or all of the alphabet letters together).

We don’t have in memory updates in functional languages. We have lists and `cons`. This is how an equivalent implementation, a dummy one, would looks like in Elixir:

<figure>
  <img width="100%" src="/images/trees/list-update.png" alt="Lists sharing">
  <figcaption>
    Function to replace every occurrence of current by new in the list. The extra variable acc is for the sake of tail recursiveness.
   </figcaption>
</figure>

This function copies every element from the old list to the new one except those that were replaced. The fact that the old list is untouched means that at every update we get a new list and preserve the old one. This property is common to every functional data structure and receives the name of *persistence*.

Since we never update a value in a list, we always build a new one, we could safely point to parts of the old list, sharing its elements. This is called *sharing* and is another nice property that trees use heavily, as we are going to see.

<figure>
  <img width="100%" src="/images/trees/persistence-sharing-list.png" alt="Lists sharing">
  <figcaption>
    Thats a hypothetical example of sharing after replacing 3 by x. As we can see, nodes 4 and 5 are shared by both lists u and v. The original list `u` remains untouched.
   </figcaption>
</figure>

But it doesn’t looks like a cleaver idea to copy lots of elements just to update a single one. How do we solve that?

## Indexed arrays using binary trees

As you were expecting, trees are one of the answers to this problem. Let fill tree nodes with indexes and we achieve logarithmic time insertions, searches, removals and updates.

They do not require a full copy to just update an element from a list. You can see a very simple binary tree bellow.

<figure>
  <img width="100%" src="/images/trees/binary-tree.png" alt="Binary tree code">
  <figcaption>
    A simple binary tree implementation.
   </figcaption>
</figure>


<figure>
  <img width="75%" src="/images/trees/sharing-trees.png" alt="Tress sharing">
  <figcaption>
    Insertions on a binary tree copies only the elements in the path to the new node, as we can see in the upper half of the image, after inserting the node 8. In the lower half of the image, we can see that if we were using a list, adding one more could mean coping the entire list.
   </figcaption>
</figure>

But its a known fact that non balanced binary trees tends to degrade to linear time after many insertions, specially if the order of insertion is always ascending, as we can see bellow:

<figure>
  <img width="50%" src="/images/trees/bad-tree.png" alt="Unbalanced tree">
  <figcaption>
    The worst case for an unbalanced tree.
   </figcaption>
</figure>

Many solutions to this problem had been presented. I remember at least three from college time. Let’s remember one of then.

## 2-3-tree

Most balancing techniques use sophisticated criteria to decide when to rearrange nodes in order to keep logarithmic tree height. These balancing strategies come in many flavors	and colors, literally.

So, in a friday afternoon <a href="https://twitter.com/carlosgaldino">@carlosgaldino</a>, <a href="fabioyamate">@fabioyamate</a> and I [implemented](https://github.com/rcillo/functional-data-structures/blob/master/lib/pfds/trees/two_three_tree.ex) a [2-3-tree](http://en.wikipedia.org/wiki/2–3_tree). It took us 4 hours to remember how that works and code it. No way we would achieve that with C code. Pattern matching provides a clear way to define scenarios. It helps focusing on the task at hand and break a big problem into smaller and simpler ones.

This is some sketching we did to understand how insertion works.

<figure>
  <img width="100%" src="/images/trees/2-3-tree-cases-1-to-5.png" alt="Some basic cases of insertion">
  <figcaption>
    Inserting from 1 to 5. Cases 3. and 5. are interesting because they contain splits.
   </figcaption>
</figure>

<figure>
  <img width="100%" src="/images/trees/2-3-tree-case-5-details.png" alt="Merging nodes">
  <figcaption>
    After inserting 5, we need to split the node with [3, 4]. But that would lead the tree to be unbalanced. So we rebalance it pushing the new node root into some existent space.
   </figcaption>
</figure>

<figure>
  <img width="100%" src="/images/trees/2-3-tree-cases-6.png" alt="Recursive root splits">
  <figcaption>
    Some times there is no room to make such merges. So we need to split the root node.
   </figcaption>
</figure>

## Performance comparison

Serious performance analysis are hard to do. Some articles claim that synthetic datasets do not lead to realistic tests. But, after so much work, we would like to take a look at how our implementation compares to a professional one, namely [Erlang gb_sets](http://www.erlang.org/doc/man/gb_sets.html).

<figure>
  <img width="100%" src="/images/trees/all_comparison.png" alt="Insertion profiling">
  <figcaption>
    Insertion time in microseconds in the vertical and dataset size in the horizontal. Datasets were specially crafted to produce binary tree worst case scenarios. Binary trees performed so badly compared to other trees that they don't even show up.
   </figcaption>
</figure>

<figure>
  <img width="100%" src="/images/trees/best_comparison.png" alt="Winners insertion comparison">
  <figcaption>
    Comparison time between 2-3-tree and gb_sets. I guess that 2-3-tree present such a weird time chart because some datasets produced few node splits. I had run the experiments once for each dataset. Maybe I should run 3 times and take the mean?
   </figcaption>
</figure>

Some may say that “C is faster” and I agree with that. But wasn’t compiling a waste of computing time? All that we need are binary instructions, isn’t it? I share [Bret Victor opinion](http://vimeo.com/71278954) regarding this subject.

## Conclusion

Relearning things from college in a new setup is a joyful experience. Even the shameful defeat for Erlang trees.

Besides been easier to think about functional code, in parts due to immutability, there are other features we gain for free:

1. Immutability makes concurrency and parallelism much easier (which actually makes it faster when you have multi cores).

2. Immutability is a language abstraction. Many languages, including Elixir and Haskell, compiles immutable code to mutable code if the compiler can guarantee there won't be any impact on the concurrent / parallelism side.

Also, being paranoid about balancing is a bad choice. Generally balanced trees are more relaxed and only run balancing operations where and when needed. That’s not a matter of hard work. Thats about doing the right work in the right time. Thats a good analogy to life.

## Next steps

I would like to implement AVL-trees, B-trees and a special one called RRB-tree which provides logarithmic merges between trees. You may find then by watching [this repo](https://github.com/rcillo/functional-data-structures). Let’s plant some trees.

### Notes

<footer>
  <ol class="foot-notes">
    <li id="fn-item1">Purely Functional Data Structures - Chris Okasaki. This book contains a much better explanation on pretty much everything I wrote in this post. This is a good computer science book.<a href="#fn-return1">↩</a></li>
  </ol>
</footer>



<!--div id="chart_div" style="width: 900px; height: 300px;"></div-->

<!--script type="text/javascript">
  google.load("visualization", "1", {packages:["corechart"]});
  google.setOnLoadCallback(drawChart);

  function drawChart() {
    var data = google.visualization.arrayToDataTable([
      /* all comparison */
      /*['Size', 'binary tree', '2-3-tree', 'gb_sets'],
      ['100', 143316, 47058, 55],
      ['200', 2845, 522, 79],
      ['300', 565687, 207494, 134],
      ['400', 10371, 1194, 106],
      ['500', 983913, 303515, 175],
      ['600', 1276979, 1870, 200],
      ['700', 1633620, 2224, 30278],
      ['800', 2053306, 372467, 313],
      ['900', 2609492, 414400, 324],
      ['1000', 3151533, 305294, 333],
      ['1100', 3529258, 411793, 30208],
      ['1200', 4219288, 4369, 378],
      ['1300', 5017978, 456298, 509],
      ['1400', 5804871, 595063, 49048],
      ['1500', 6456788, 498307, 828],
      ['1600', 163070, 513166, 49128],
      ['1700', 8141241, 6124, 632],
      ['1800', 9116663, 159786, 67629],
      ['1900', 10018652, 753333, 686],
      ['2000', 10926524, 8026, 30266],
      ['2100', 11933849, 615409, 67458],
      ['2200', 12957195, 620116, 904],
      ['2300', 14160245, 664019, 814],
      ['2400', 15418879, 216201, 67523],
      ['2500', 16700407, 262735, 1205],
      ['2600', 17703181, 939057, 30516],
      ['2700', 19137663, 313092, 30478],
      ['2800', 21470544, 163624, 91782],
      ['2900', 23463085, 750060, 1150],
      ['3000', 24943499, 1054293, 1298],
      ['3100', 26966578, 120531, 91672],
      ['3200', 10785407, 12959, 91784],
      ['3300', 30470883, 837073, 92478],
      ['3400', 31251740, 1163678, 1852],
      ['3500', 33114191, 844285, 123671],
      ['3600', 36062537, 1218730, 31428],
      ['3700', 38244953, 901302, 64066],
      ['3800', 39808916, 317487, 1295],
      ['3900', 41831144, 1312461, 34097],
      ['4000', 45308395, 916620, 1817],
      ['4100', 47085096, 958673, 80793],
      ['4200', 30984204, 1373176, 77550],
      ['4300', 50668992, 1405724, 43373],
      ['4400', 53041696, 271680, 44059],
      ['4500', 57542539, 983477, 139063],
      ['4600', 59666641, 18354, 121112],
      ['4700', 60344449, 1051355, 1905],
      ['4800', 62773811, 1517042, 2592],
      ['4900', 67264337, 1581664, 62059],
      ['5000', 69705758, 1053465, 123324],
      ['5100', 70311859, 1108337, 61498],
      ['5200', 75844412, 1640084, 173570],
      ['5300', 77953117, 176430, 2406],
      ['5400', 80442590, 1701067, 149378],
      ['5500', 80886743, 1110252, 72172],
      ['5600', 83537482, 24483, 156850],
      ['5700', 89607599, 1182041, 177150],
      ['5800', 92217939, 1775332, 75218],
      ['5900', 92533446, 1793310, 163133],
      ['6000', 95741976, 1160936, 2229],
      ['6100', 102673216, 1225680, 81239],
      ['6200', 104924261, 1843069, 206913],
      ['6300', 105397769, 74258, 186453],
      ['6400', 108429959, 1888921, 82666],
      ['6500', 115414330, 1212738, 233516],
      ['6600', 114375321, 1285093, 3532],
      ['6700', 120441372, 1927714, 90845],
      ['6800', 120604474, 1949096, 191666],
      ['6900', 128464277, 1253756, 85998],
      ['7000', 130509549, 1332268, 212382],
      ['7100', 130707661, 241539, 3250],
      ['7200', 136797074, 2008134, 216168],
      ['7300', 142206108, 2034170, 90481],
      ['7400', 142879446, 1302128, 93983],
      ['7500', 143682790, 1387248, 237297],
      ['7600', 147132601, 287424, 3395],
      ['7700', 155545906, 2097409, 216436],
      ['7800', 155156676, 2117901, 90846],
      ['7900', 157237839, 243554, 237258],
      ['8000', 160584459, 1364775, 101860],
      ['8100', 169845661, 291137, 239036],
      ['8200', 173742291, 1474648, 254562],
      ['8300', 171386703, 2183009, 104321],
      ['8400', 174822714, 193063, 269687],
      ['8500', 184255341, 1405753, 270403],
      ['8600', 181548134, 948294, 286497],
      ['8700', 187486246, 1535327, 115864],
      ['8800', 188563652, 2258285, 104456],
      ['8900', 200213784, 2269515, 4187],
      ['9000', 199511685, 1443656, 4222],
      ['9100', 207666202, 40725, 100200],
      ['9200', 203914497, 1593051, 239089],
      ['9300', 216275750, 2318839, 255006],
      ['9400', 215405292, 1477303, 239420],
      ['9500', 216082126, 2343215, 255459],
      ['9600', 220302969, 1634332, 109041],
      ['9700', 233383942, 2357908, 104828],
      ['9800', 232120128, 1542890, 256047],
      ['9900', 232342676, 2384702, 3596],
      ['10000', 236300305, 151878, 118743],
      ['10100', 249917201, 200180, 284759],
      ['10200', 246407988, 1694200, 270013],
      ['10300', 248441598, 2425294, 270560],
      ['10400', 252424459, 1658204, 104526],
      ['10500', 266997271, 2464457, 271222],
      ['10600', 266093798, 1733147, 118874],
      ['10700', 264076701, 1970501, 283745],
      ['10800', 268000256, 2504334, 271289],
      ['10900', 283128087, 1737415, 114188],
      ['11000', 283301884, 1765137, 122757],
      ['11100', 279991828, 2535680, 283714],
      ['11200', 294807861, 2546479, 283948],
      ['11300', 287922189, 1794267, 114254],
      ['11400', 298975027, 2572262, 132548],
      ['11500', 306056286, 1800405, 291173],
      ['11600', 306365486, 2591813, 293115],
      ['11700', 314733587, 2594301, 124491],
      ['11800', 316053859, 1819855, 125055],
      ['11900', 316917192, 1564443, 284721],
      ['12000', 323347223, 1880833, 6687],
      ['12100', 330487201, 2615963, 142159],
      ['12200', 328548753, 60090, 286395],
      ['12300', 333838159, 2628105, 132772],
      ['12400', 341667367, 1864554, 6576],
      ['12500', 339890185, 2638013, 293665],
      ['12600', 343882819, 1966216, 5397],
      ['12700', 348257155, 2039745, 295137],
      ['12800', 339639454, 1892294, 134575],
      ['12900', 353992541, 2049264, 125465],
      ['13000', 357505757, 2015574, 290622],
      ['13100', 365550334, 2604380, 294234],
      ['13200', 364602510, 1910287, 134641],
      ['13300', 367319308, 2619962, 138206],
      ['13400', 374786957, 2628346, 132177],
      ['13500', 373691954, 2638595, 291776],
      ['13600', 382909102, 887042, 284616],
      ['13700', 381419332, 2654480, 127461],
      ['13800', 382992256, 125674, 137573],
      ['13900', 382170717, 123841, 285930],
      ['14000', 391134752, 2673215, 284936],
      ['14100', 392070831, 2685166, 127324],
      ['14200', 401860751, 2970634, 127782],
      ['14300', 401118764, 2698139, 136674],
      ['14400', 400927566, 3030997, 287041],
      ['14500', 406313707, 3104959, 291395],
      ['14600', 411770309, 2477611, 136957],
      ['14700', 409851094, 2286017, 291719],
      ['14800', 420631757, 3107102, 151278],
      ['14900', 420154212, 2289116, 212517],
      ['15000', 418349224, 2872711, 180547],
      ['15100', 423033144, 2748243, 268059],
      ['15200', 429819586, 2291565, 5450],
      ['15300', 426856795, 2532697, 164655],
      ['15400', 438185707, 2284248, 249506],
      ['15500', 439237191, 2886744, 7381],
      ['15600', 435425288, 231540, 181135],
      ['15700', 440455064, 76861, 256865],
      ['15800', 445115927, 2893788, 169036],
      ['15900', 443611227, 2894775, 6707],
      ['16000', 457095439, 2894514, 186756],
      ['16100', 454193975, 2892566, 155061],
      ['16200', 451546942, 3143223, 169908],
      ['16300', 458229933, 3090577, 185305],
      ['16400', 463945612, 3087603, 270368],
      ['16500', 459391496, 3139973, 140844],
      ['16600', 464001476, 3145920, 244333],
      ['16700', 469365104, 2426894, 142530],
      ['16800', 467095268, 3056932, 190584],
      ['16900', 473050125, 3127041, 277407],
      ['17000', 475790026, 3126762, 191462],
      ['17100', 472811388, 3083771, 271266],
      ['17200', 479672605, 2996104, 227669],
      ['17300', 481297721, 2972835, 172328],
      ['17400', 489460630, 3006121, 173699],
      ['17500', 484010990, 2604124, 214407],
      ['17600', 486436033, 3018611, 197416],
      ['17700', 487776813, 3147353, 249864],
      ['17800', 490880800, 2937784, 196360],
      ['17900', 491847322, 2946114, 267391],
      ['18000', 498093702, 3061472, 155823],
      ['18100', 494877339, 3119681, 270301],
      ['18200', 503937094, 3045629, 129435],
      ['18300', 505268548, 2350445, 237593],
      ['18400', 503283704, 2868493, 201849],
      ['18500', 507024356, 2872038, 259371],
      ['18600', 500405878, 2873857, 202535],
      ['18700', 504282463, 2875024, 216861],
      ['18800', 508744963, 2876168, 148317],
      ['18900', 508249240, 2902867, 247228],
      ['19000', 505786110, 2906568, 220640],
      ['19100', 508054231, 2160620, 240628],
      ['19200', 507280884, 2688875, 231958],
      ['19300', 511440934, 2912000, 232036],
      ['19400', 510587636, 2173542, 207462],
      ['19500', 511870781, 2712288, 260597],
      ['19600', 510240597, 2915840, 266197],
      ['19700', 511287973, 2183781, 252653],
      ['19800', 511440325, 2734276, 208433],
      ['19900', 512197455, 2918742, 159145],
      ['20000', 510477959, 2192755, 223375]*/

      ['Size', '2-3-tree', 'gb_sets'],
      ['100', 47058, 55],
      ['200', 522, 79],
      ['300', 207494, 134],
      ['400', 1194, 106],
      ['500', 303515, 175],
      ['600', 1870, 200],
      ['700', 2224, 30278],
      ['800', 372467, 313],
      ['900', 414400, 324],
      ['1000', 305294, 333],
      ['1100', 411793, 30208],
      ['1200', 4369, 378],
      ['1300', 456298, 509],
      ['1400', 595063, 49048],
      ['1500', 498307, 828],
      ['1600', 513166, 49128],
      ['1700', 6124, 632],
      ['1800', 159786, 67629],
      ['1900', 753333, 686],
      ['2000', 8026, 30266],
      ['2100', 615409, 67458],
      ['2200', 620116, 904],
      ['2300', 664019, 814],
      ['2400', 216201, 67523],
      ['2500', 262735, 1205],
      ['2600', 939057, 30516],
      ['2700', 313092, 30478],
      ['2800', 163624, 91782],
      ['2900', 750060, 1150],
      ['3000', 1054293, 1298],
      ['3100', 120531, 91672],
      ['3200', 12959, 91784],
      ['3300', 837073, 92478],
      ['3400', 1163678, 1852],
      ['3500', 844285, 123671],
      ['3600', 1218730, 31428],
      ['3700', 901302, 64066],
      ['3800', 317487, 1295],
      ['3900', 1312461, 34097],
      ['4000', 916620, 1817],
      ['4100', 958673, 80793],
      ['4200', 1373176, 77550],
      ['4300', 1405724, 43373],
      ['4400', 271680, 44059],
      ['4500', 983477, 139063],
      ['4600', 18354, 121112],
      ['4700', 1051355, 1905],
      ['4800', 1517042, 2592],
      ['4900', 1581664, 62059],
      ['5000', 1053465, 123324],
      ['5100', 1108337, 61498],
      ['5200', 1640084, 173570],
      ['5300', 176430, 2406],
      ['5400', 1701067, 149378],
      ['5500', 1110252, 72172],
      ['5600', 24483, 156850],
      ['5700', 1182041, 177150],
      ['5800', 1775332, 75218],
      ['5900', 1793310, 163133],
      ['6000', 1160936, 2229],
      ['6100', 1225680, 81239],
      ['6200', 1843069, 206913],
      ['6300', 74258, 186453],
      ['6400', 1888921, 82666],
      ['6500', 1212738, 233516],
      ['6600', 1285093, 3532],
      ['6700', 1927714, 90845],
      ['6800', 1949096, 191666],
      ['6900', 1253756, 85998],
      ['7000', 1332268, 212382],
      ['7100', 241539, 3250],
      ['7200', 2008134, 216168],
      ['7300', 2034170, 90481],
      ['7400', 1302128, 93983],
      ['7500', 1387248, 237297],
      ['7600', 287424, 3395],
      ['7700', 2097409, 216436],
      ['7800', 2117901, 90846],
      ['7900', 243554, 237258],
      ['8000', 1364775, 101860],
      ['8100', 291137, 239036],
      ['8200', 1474648, 254562],
      ['8300', 2183009, 104321],
      ['8400', 193063, 269687],
      ['8500', 1405753, 270403],
      ['8600', 948294, 286497],
      ['8700', 1535327, 115864],
      ['8800', 2258285, 104456],
      ['8900', 2269515, 4187],
      ['9000', 1443656, 4222],
      ['9100', 40725, 100200],
      ['9200', 1593051, 239089],
      ['9300', 2318839, 255006],
      ['9400', 1477303, 239420],
      ['9500', 2343215, 255459],
      ['9600', 1634332, 109041],
      ['9700', 2357908, 104828],
      ['9800', 1542890, 256047],
      ['9900', 2384702, 3596],
      ['10000', 151878, 118743],
      ['10100', 200180, 284759],
      ['10200', 1694200, 270013],
      ['10300', 2425294, 270560],
      ['10400', 1658204, 104526],
      ['10500', 2464457, 271222],
      ['10600', 1733147, 118874],
      ['10700', 1970501, 283745],
      ['10800', 2504334, 271289],
      ['10900', 1737415, 114188],
      ['11000', 1765137, 122757],
      ['11100', 2535680, 283714],
      ['11200', 2546479, 283948],
      ['11300', 1794267, 114254],
      ['11400', 2572262, 132548],
      ['11500', 1800405, 291173],
      ['11600', 2591813, 293115],
      ['11700', 2594301, 124491],
      ['11800', 1819855, 125055],
      ['11900', 1564443, 284721],
      ['12000', 1880833, 6687],
      ['12100', 2615963, 142159],
      ['12200', 60090, 286395],
      ['12300', 2628105, 132772],
      ['12400', 1864554, 6576],
      ['12500', 2638013, 293665],
      ['12600', 1966216, 5397],
      ['12700', 2039745, 295137],
      ['12800', 1892294, 134575],
      ['12900', 2049264, 125465],
      ['13000', 2015574, 290622],
      ['13100', 2604380, 294234],
      ['13200', 1910287, 134641],
      ['13300', 2619962, 138206],
      ['13400', 2628346, 132177],
      ['13500', 2638595, 291776],
      ['13600', 887042, 284616],
      ['13700', 2654480, 127461],
      ['13800', 125674, 137573],
      ['13900', 123841, 285930],
      ['14000', 2673215, 284936],
      ['14100', 2685166, 127324],
      ['14200', 2970634, 127782],
      ['14300', 2698139, 136674],
      ['14400', 3030997, 287041],
      ['14500', 3104959, 291395],
      ['14600', 2477611, 136957],
      ['14700', 2286017, 291719],
      ['14800', 3107102, 151278],
      ['14900', 2289116, 212517],
      ['15000', 2872711, 180547],
      ['15100', 2748243, 268059],
      ['15200', 2291565, 5450],
      ['15300', 2532697, 164655],
      ['15400', 2284248, 249506],
      ['15500', 2886744, 7381],
      ['15600', 231540, 181135],
      ['15700', 76861, 256865],
      ['15800', 2893788, 169036],
      ['15900', 2894775, 6707],
      ['16000', 2894514, 186756],
      ['16100', 2892566, 155061],
      ['16200', 3143223, 169908],
      ['16300', 3090577, 185305],
      ['16400', 3087603, 270368],
      ['16500', 3139973, 140844],
      ['16600', 3145920, 244333],
      ['16700', 2426894, 142530],
      ['16800', 3056932, 190584],
      ['16900', 3127041, 277407],
      ['17000', 3126762, 191462],
      ['17100', 3083771, 271266],
      ['17200', 2996104, 227669],
      ['17300', 2972835, 172328],
      ['17400', 3006121, 173699],
      ['17500', 2604124, 214407],
      ['17600', 3018611, 197416],
      ['17700', 3147353, 249864],
      ['17800', 2937784, 196360],
      ['17900', 2946114, 267391],
      ['18000', 3061472, 155823],
      ['18100', 3119681, 270301],
      ['18200', 3045629, 129435],
      ['18300', 2350445, 237593],
      ['18400', 2868493, 201849],
      ['18500', 2872038, 259371],
      ['18600', 2873857, 202535],
      ['18700', 2875024, 216861],
      ['18800', 2876168, 148317],
      ['18900', 2902867, 247228],
      ['19000', 2906568, 220640],
      ['19100', 2160620, 240628],
      ['19200', 2688875, 231958],
      ['19300', 2912000, 232036],
      ['19400', 2173542, 207462],
      ['19500', 2712288, 260597],
      ['19600', 2915840, 266197],
      ['19700', 2183781, 252653],
      ['19800', 2734276, 208433],
      ['19900', 2918742, 159145],
      ['20000', 2192755, 223375]
    ]);

    var options = {
      // curveType: 'function'
    };

    var chart = new google.visualization.LineChart(document.getElementById('chart_div'));
    chart.draw(data, options);
  }
</script-->
