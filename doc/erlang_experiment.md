```
MartinWork:erlang martinsumner$ erl -pa ebin
Erlang/OTP 19 [erts-8.3] [source] [64-bit] [smp:8:8] [async-threads:10] [kernel-poll:false]

Eshell V8.3  (abort with ^G)
1> DB0 = discodb:new([]).
<<>>
2> discodb:add(discodb:cons(DB0), [{"Key1", "Value1"}], [unique_items]).
{error,badarg}
3> discodb:add(discodb:cons(DB0), [{"Key1", "Value1"}]).
{error,badarg}
4> DB1 = discodb:new([]).
<<>>
5> discodb:add(discodb:cons(DB1), "Key1", "Value1").
<<>>
6> DB1F = discodb:finalize(DB1).
{error,badarg}
7> DB1F = discodb:finalize(DB1, [unique_items]).
{error,badarg}
8> DB2 = discodb:new([{"key1", "v1"}], [unique_items]).
<<>>
9> DB2F = discodb:finalize(DB2, [unique_items]).
{error,badarg}
10> DB2F = discodb:finalize(DB2).
{error,badarg}
11> DB3 = discodb:new([]).
<<>>
12> discodb:keys(DB3).
<<>>
13> discodb:add(discodb:cons(DB3), "Key1", "Value1").
<<>>
14> discodb:keys(DB3).
<<>>
15> discodb:finalize(DB3).
{error,badarg}
16> DB4 = discodb:new([{"key1", "v1"}]).
<<>>
17> discodb:keys(DB4).
<<>>
18> discodb:size(discodb:keys(DB4)).
1
19> discodb:size(discodb:keys(DB3)).
0
20> discodb:get(DB4, "key1").
<<>>
21> K1 = discodb:get(DB4, "key1").
<<>>
22> K1.
<<>>
23> K1 = discodb:peek(DB4, get, "key1").
** exception error: no match of right hand side value <<"v1">>
24> V1 = discodb:peek(DB4, get, "key1").
<<"v1">>
25> KL = discodb:peek(DB4, keys).
** exception error: no case clause matching {error,badarg}
     in function  discodb:peek/2 (src/discodb.erl, line 178)
26> KL = discodb:peek(DB4, keys, []).
** exception error: no case clause matching {error,badarg}
     in function  discodb:peek/2 (src/discodb.erl, line 178)
27> KL = discodb:peek(DB4, iter, keys).
<<"key1">>
28> discodb:add(DB4, "key2", "v1").
{error,badarg}
29> discodb:add(discodb:cons(DB4), "key2", "v1").
<<>>
30> KL2 = discodb:peek(DB4, iter, keys).
<<"key1">>
31> V2 = discodb:peek(DB4, get, "key2").
null
32> discodb:finalize(DB4).
{error,badarg}
33> discodb:finalize(discodb:add(discodb:cons(DB4), "key2", "v1")).
<<>>
34> V2A = discodb:peek(DB4, get, "key2").
null
35> DB5 = discodb:finalize(discodb:add(discodb:cons(DB4), "key2", "v1")).
<<>>
36> V2B = discodb:peek(DB5, get, "key2").
<<"v1">>
37> V2C = discodb:list(DB5, get, "key2").
[<<"v1">>]
38> DB6 = discdb:add(discodb:cons(DB5), "key2", "v2").
** exception error: undefined function discdb:add/3
39> DB6 = discodb:add(discodb:cons(DB5), "key2", "v2").
<<>>
40> discodb:list(DB5, get, "key2").
[<<"v1">>]
41> discodb:list(DB6, get, "key2").
** exception error: no case clause matching {error,badarg}
     in function  discodb:fold/3 (src/discodb.erl, line 138)
     in call from discodb:to_list/1 (src/discodb.erl, line 155)
42> DB7 = discodb:finalize(DB6).
<<>>
43> discodb:list(DB7, get, "key2").
[<<"v1">>,<<"v2">>]
44> discodb:list(DB5, get, "key2").
[<<"v1">>]
45> discodb:list(DB6, get, "key2").
** exception error: no case clause matching {error,badarg}
     in function  discodb:fold/3 (src/discodb.erl, line 138)
     in call from discodb:to_list/1 (src/discodb.erl, line 155)
46> discodb:peek(DB6, get, "key2").
** exception error: no case clause matching {error,badarg}
     in function  discodb:peek/2 (src/discodb.erl, line 178)
47> DB7 = discodb:finalize(discodb:add(discodb:cons(DB6), <<"key3">>, <<"v2">>)).
** exception error: no match of right hand side value {error,badarg}
48> DB8 = discodb:finalize(discodb:add(discodb:cons(DB7), <<"key3">>, <<"v2">>)).
<<>>
49> discodb:list(DB8, get, "key3").
[<<"v2">>]
50> discodb:q(DB8, "key2|key3").
{error,badarg}
51> discodb:q(DB8, "key2 | key3").
{error,badarg}
52> discodb:q(DB8, '<<"key2">> | <<"key3">>').
{error,badarg}
53> discodb:q(DB8, '"key2" | "key3"').
{error,badarg}
54> discodb:q(discodb:cons(DB8), '"key2" | "key3"').
{error,badarg}
55> discodb:q(discodb:cons(DB8), <<"key2 | key3">>).
{error,badarg}
56> discodb:q(DB8, <<"key2 | key3">>).
{error,badarg}
57> discodb:query(DB8, <<"key2 | key3">>).
{error,badarg}
58> discodb:q(DB8, <<"key3">>).
{error,badarg}
59> discodb:q(DB8, [<<"key3">>]).
{error,badarg}
60> discodb:q(DB8, "key3").
{error,badarg}
61> discodb:list(DB8, get, "key3").
[<<"v2">>]
62> discodb:q(discodb:cons(DB8), "key3").
{error,badarg}
63> discodb:q(DB8, "key3").
{error,badarg}
64> discodb:q(DB8, ["key3"]).
{error,badarg}
65> discodb:query(DB8, ["key3"]).
{error,badarg}
66> discodb:q(DB8, {"key3"}).
{error,badarg}
67> discodb:q(DB8, "").
<<>>
68> discodb:q(DB8, []).
<<>>
69> discodb:q(DB8, {}).
{error,badarg}
70> discodb:q(DB8, [<<"key3">>]).
{error,badarg}
71> discodb:q(DB8, [[<<"key3">>]]).
<<>>
72> discodb:q(DB8, [[<<"key3">>], <<"|">>, <<"key2">>]).
{error,badarg}
73> discodb:q(DB8, [[<<"key3">>, <<"|">>, <<"key2">>]]).
<<>>
74> discodb:list(discodb:q(DB8, [[<<"key3">>, <<"|">>, <<"key2">>]])).
[<<"v1">>,<<"v2">>]
75>
```

this better explains querying

```
23> discodb:list(discodb:q(DB5, [[<<"K1">>], [<<"K3">>]])).        
[<<"V1">>]
24> discodb:list(discodb:q(DB5, [[<<"K1">>], [<<"K2">>]])).
[<<"V2">>]
25> discodb:list(discodb:q(DB5, [[<<"K1">>, <<"K2">>]])).  
[<<"V1">>,<<"V2">>]
26> discodb:list(discodb:q(DB5, [[<<"K1">>, {no, <<"K2">>}]])).
[<<"V1">>,<<"V2">>,<<"V3">>]
27>
```

Inner and outer lists are used for or and and, and {no, K} as a tuple is used for NOT.
