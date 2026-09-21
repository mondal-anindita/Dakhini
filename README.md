# Spontaneous Dialect-Aware Speech Corpus for Low-Resource Dakhini

**A Southern Indo-Aryan Language: Methods, Challenges, and Insights**

Anindita Mondal<sup>1,\*</sup>, Priyanka Kommagouni<sup>2,\*</sup>

<sup>1</sup> Language Technologies Research Center, IIIT Hyderabad, India
<sup>2</sup> Independent Researcher
<sup>\*</sup> Equal contribution

Accepted at **Interspeech 2026**, ICC Sydney, Australia, 28 September – 1 October 2026.

**Demo page:** https://USERNAME.github.io/REPO/ — *(replace once Pages is live)*

---

## About

Dakhini is a contact variety spoken across the Deccan region of southern India, blending Persian,
Old Urdu (Dehlavi), Kannada, Marathi and Telugu. It is in widespread conversational use and almost
entirely absent from curated speech resources.

This repository hosts the demo page for our Interspeech 2026 paper, which presents a data-centric
methodology for building a Dakhini speech corpus. The central problem is not acoustic but
sociolinguistic: Dakhini has no formal written standard and carries an internalised stigma among
its own speakers, so a microphone and an institutional framing reliably cause speakers to produce a
more standard variety than they otherwise would. A recording that is acoustically clean but
linguistically inauthentic is worse than useless for dialect documentation.

The paper's response has three parts:

- **An asymmetric awareness protocol.** Recordings are telephonic conversations between pairs who
  already know each other. Only one participant i.e. the *anchor speaker* knows about the recording
  in advance and steers the conversation toward a loose menu of casual topics. The other
  participant is fully debriefed afterwards, and audio is retained only on explicit consent.
- **Telephony over studio.** Reduced signal fidelity is accepted deliberately in exchange for
  linguistic authenticity, on the argument that for dialect documentation authenticity is the more
  consequential variable.
- **Dialect-aware annotation.** Standard Hindi/Urdu ASR systematically normalises Dakhini forms
  away. Rather than treating those mismatches as transcription errors, the pipeline treats
  divergence between ASR output and audio as a *positive dialect signal* and flags it for
  rule-based tagging.

## Corpus overview

| | |
|---|---|
| Speakers | 160, from Hyderabad, India |
| Early-settler families | ~10% |
| Age range | 17–50 |
| Gender distribution | 70% male, 30% female |
| Higher educational attainment | ~20% |
| Upper socioeconomic strata | ~20% |
| Languages spoken | Dakhini, Urdu, Telugu, English (varying proficiency) |
| Recording modality | Telephonic |
| Session length | ~20 minutes |
| Retained per session | ~2-5 minutes of representative conversational speech |
| Pair composition | One male, one female speaker, already familiar with each other |

The mixed-gender pair design serves two purposes at once: it reflects naturalistic interaction in
Dakhini-speaking communities, and the acoustic contrast between male and female voices improves
diarization reliability on spontaneous speech.

## Annotation tags

Rule-based detection modules derived from prior structural analysis of Dakhini:

| Tag | Module | Standard Hindi/Urdu | Dakhini realisation |
|---|---|---|---|
| `PA` | Phonological assimilation | *itnaa*, *us zamaane* | *ittaa*, *uzzamaane* |
| `PV` | Vowel-length reduction | *aadmii*, *acchaa*, *bataao* | *admii*, *accha*, *batau* |
| `MAUX` | Auxiliary omission | *woh gaye hai*, *usne diye the* | *woh gaye*, *une diye* |
| `MP-KO` | Participial *ko* for *kar* | *main khaa kar aaya* | *main khaa ko aaya* |
| `MP` | Pronoun variation | *mujh se*, *unheN*, *inheN* | *mere se*, *une*, *ine* |
| `SF` | Formality register marker | *aap* | *tum* / *tu* |
| `LD` | Dakhini-specific lexical item | *kyon*, *mat karo*, *mein*, *dhire se* | *kaiku*, *nakko karo*, *manjhe*, *haule* |

`MAUX` requires parse-level rather than string-level matching. Segments flagged on several rules at
once are prioritised for detailed human review; segments with no flags pass through with lighter
review, concentrating annotator effort where it matters most.

## Pipeline

```
Telephonic recording
        │
        ▼
  Diarization ──── pyannote speaker-diarization-3.1
        │           ECAPA-TDNN embeddings, trained on VoxCeleb
        │           + manual verification of boundaries and attributions
        ▼
  Anchor speaker removed; target-speaker segments concatenated
  (amplitude ramp at segment boundaries to avoid audible artefacts)
        │
        ▼
  ASR pre-transcription ──── IndicConformer Hindi ASR
        │
        ▼
  Rule-based tagging ──── PA · PV · MAUX · MP-KO · MP · SF · LD
        │
        ▼
  Human-in-the-loop review, triaged by tag density
        │
        ▼
  Validated speaker-level annotated corpus
```

Corrections from human validation are fed back into the automated models, so the pipeline improves
on dialectal speech over successive passes.

## Repository contents

```
.
├── index.html        Demo page (GitHub Pages entry point)
├── README.md         This file
├── paper.pdf         Camera-ready paper
├── poster.pdf        Interspeech 2026 poster
└── audio/            Audio samples featured on the demo page
    ├── dakhini_01.wav
    ├── dakhini_02.wav
    └── ...
```


## Ethics and consent

Participation was collective and informed. Recruitment ran through community information sessions
rather than individual solicitation, working with trusted neighbourhood intermediaries in the older
Hyderabad neighbourhoods where intergenerational dialect transmission remains intact.

Following each conversation, the uninformed participant was fully debriefed: the research intent was
explained, and informed consent was sought explicitly **before any data was retained**. Sessions
where consent was withheld were discarded in their entirety, with no data kept. Participants were
informed of their right to withdraw their data at any stage. After consent, diarization was applied
to retain only the target speaker's audio; the anchor speaker's audio was excluded from the primary
corpus.

The covert-recording-with-retrospective-consent design follows established sociolinguistic practice
grounded in Labov's work, which showed that prior awareness of recording systematically alters
speaker behaviour in ways that undermine dialectal authenticity.


## Citation

```bibtex
@inproceedings{2026dakhini,
  title     = {Spontaneous Dialect-Aware Speech Corpus for Low-Resource Dakhini,
               A Southern Indo-Aryan Language: Methods, Challenges, and Insights},
  author    = {Mondal, Anindita and Kommagouni, Priyanka},
  booktitle = {Proc. Interspeech 2026},
  year      = {2026},
  address   = {Sydney, Australia}
}
```

## Acknowledgments

We thank IIIT Hyderabad for institutional support and for providing the resources necessary for this
work; the individuals who facilitated access to local communities and helped establish the contacts
essential for data collection; the community intermediaries whose support ensured the data
collection process ran smoothly; and above all the participants, whose voices form the core of this
corpus.

## Contact

Anindita Mondal — anindita.mondal@research.iiit.ac.in

Priyanka Kommagouni — parvathipriyanka86@gmail.com
