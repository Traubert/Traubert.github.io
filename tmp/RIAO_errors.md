# Debugging UD_Akkadian-RIAO

Due to UD increasing the strictness of their rules, two new error classes are now blockers for Akkadian-RIAO to be included in the UD repertoire. I am basically guessing as to what the correct analysis for them is, and include my proposals that would technically fix the problem, but require a check from an expert.

## _lā kanšūtešu_, _lā māgirīšu_ and others

The corpus has some multiword tokens where the first part of the multiword token contains a space, and UD now complains about these. It does _not_ appear to complain about similar cases where there is no multiword token, but I think it probably should / will, so these should be fixed also.

The complaint it gives now looks like:

```
[Line 7574 Sent Q004456-1]: [L1 Format invalid-whitespace-mwt] White space not allowed in multi-word token 'lā kanšūtešu'. If it contains a space, it is not one surface token.
```

What this looks like in CoNLL-U:


```
73-74	lā kanšūtešu	_	_	_	_	_	_	_	_
73	lā kanšūte	lā|kanšu	NOUN	MOD|N	Gender=Masc|NounBase=Suffixal|Number=Plur	72	nmod:poss	_	la|kan-šu-te-šu₂
74	šu	_	PRON	_	Gender=Masc|Number=Sing|Person=3	
```

What this whole sentence is:

ēkal Aššur-naṣir-apli iššak Aššur nišīt Enlil u Ninurta narām Anim u Dagan kašūš ilāni rabûti šarru dannu šar kiššati šar māt Aššur mār Tukulti-Ninurta šarri rabê šarri danni šar kiššati šar māt Aššur mār Adad-nerari šar kiššati šar māt Aššurma eṭlu qardu ša ina tukulti Aššur bēlišu ittallakuma ina malkī ša kibrāt erbetta šāninšu lā īšû rēʾû tabrâte lā ādiru tuqumti edû gapšu ša māhira lā īšû šar mušakniš *lā kanšūtešu* ša naphar kiššat nišī ipēlu zikaru dannu mukabbis kišād ayyābīšu dāʾiš kullat nakirē muparriru kiṣir multarhī šarru ša ina tukulti ilāni rabûti bēlēšu ittallakuma mātāti kalîšina qāssu ikšud huršānī kalûšunu ipēluma bilatsunu imhuru ṣābit līṭē šākin līte

Other cases where this is probably also a problem: lā mēna, lā pādû, lā māgirī, lā šanān, lā ādiru etc. We must reanalyse these forms as two tokens with some syntactic relationship between them.

From annotations (in eg. ORACC) it appears that lā is a negating particle. Is that correct? In that case *I propose that we annotate all these with [neg](https://universaldependencies.org/docs/u/dep/neg.html)*.

The corpus contains a total of 62 cases of lā being analysed as part of one token together with another word. It also contains 112 cases of lā being analysed as its own token. In those cases, it is generally analysed as a modifying particle (MOD and PART) with the syntactic relationship [advmod](https://universaldependencies.org/u/dep/advmod.html) to its head, and sometimes [obl](https://universaldependencies.org/u/dep/obl.html), sometimes [conj](https://universaldependencies.org/u/dep/conj.html), sometimes [nmod:poss](https://universaldependencies.org/u/dep/nmod-poss.html).

*I propose that we change the annotation in these cases of `advmod` to `neg`.*

## Clauses with multiple subjects

There are two clauses with more than one subject in the corpus, and the validation says

```
[Line 2093 Sent Q004457-1 Node 15]: [L3 Syntax too-many-subjects] Node has multiple subjects not subtyped as ':outer': [11, 16]. Outer subjects are allowed if a clause acts as the predicate of another clause.
```

This looks like:

```
11	ša	ša	PRON	REL	_	15	nsubj	_	ša₂
12	ina	ina	ADP	PRP	_	13	case	_	ina
13	tāhāzi	tāhāzu	NOUN	N	Case=Gen|Gender=Masc|NounBase=Free|Number=Sing	15	obl	_	ME₃
14	lā	lā	PART	MOD	_	15	advmod	_	la-a
15	iššannanu	šanānu	VERB	V	Gender=Masc|Mood=Ind|Number=Sing|Person=3|Subordinative=Yes|Tense=Pres|VerbForm=Fin|VerbStem=N	2	acl:relcl	_	iš-ša₂-na-nu
16-17	tībušu	_	_	_	_	_	_	_	_
16	tību	tību	NOUN	N	Case=Nom|Gender=Masc|NounBase=Suffixal|Number=Sing	15	nsubj	_	ti-bu-šu₂
17	šu	_	PRON	_	Gender=Masc|Number=Sing|Person=3	
```

And the whole sentence is


ana Ninurta gešri dandanni ṣīri ašarēd ilāni qardu šarhu gitmālu *ša ina tāhāzi lā iššannanu tībušu* aplu rēštû hāmim tuqmāte bukur Nudimmud qurād Igigi lēʾû malik ilāni ilitti Ekur mukīl markas šamê erṣeti pētû nagbē kābisi erṣeti rapašti ilu ša ina baluššu purussê šamê u erṣeti lā ipparrasu munnarbu ekdu ša lā enû qibīt pîšu ašarēd kibrāti nādin haṭṭi u purussê ana naphar kal ālāni gugallu šamru ša lā uttakkaru siqir šaptišu lēʾû rapšu apkal ilāni muttallu Utulu bēl bēlē ša kippat šamê u erṣeti qātuššu paqdu šar tamhāri ālilu ša tuqmatu ītallu šulluṭu gitmālu bēl nagbē u tâmāti ezzu lā pādû ša tībušu abūbu sāpin māt nakirē mušamqit targīgī ilu šarhu ša lā enû ištiššu nūr šamê erṣeti mušpardû qereb apsî muʾabbit lemnūti mušakniš lā māgirī muhalliq zayyārī ša ina puhur ilāni siqiršu ilu mamma lā innû qā’iš balāṭi ilu rēmēnû ša sīpûšu ṭāb āšib Kalhi bēli rabê bēliya


This is about the pronoun _ša_ and the noun _tībušu_ being marked as subjects in the indicative verb _iššannanu_. Is _ša_ here in fact a [relativizer](https://universaldependencies.org/workgroups/newdoc/relative_clauses.html)? If so, is it perhaps a possessive modifier or some other kind of modifier? Elsewhere in the corpus, when it is a PRON and REL, it has always been syntactically analysed as either subject, object or oblique nominal. Or we could analyse _ša_ as the subject and _tībušu_ as the [relative clause modifier](https://universaldependencies.org/u/dep/acl-relcl.html), which would technically work, but would probably not be true to the intended meaning.

I really need an opinion from someone, but absent one, *I propose that in these cases, we change the syntactic role of _ša_ to `obl`*.
