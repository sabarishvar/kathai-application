# 2.3 Results

Model: facebook/nllb-200-distilled-600M, Tamil to English.


## 1) Following one sentence through the model

Sentence: நான் நேற்று சென்னையில் 42 ரூபாய்க்கு மீன் வாங்கினேன்.
Output: "I bought fish for Rs 42 yesterday in Chennai." This is correct.

- 7 Tamil words became 16 pieces (18 with the language tag and end marker).
- சென்னையில் (in Chennai) was split into 3 pieces: செ | ன்ன | ையில்
- ரூபாய்க்கு (for rupees) was split into 5 pieces: ரூ | பா | ய | ்க்க | ு
- Some pieces start with a vowel sign, so the cut is inside a Tamil letter.
- The encoder turns the 18 pieces into 18 vectors of 1,024 numbers each.
- The decoder changed the word order from Tamil (verb last) to English. The one word ரூபாய்க்கு became two English words, "for Rs".
- Many chosen words had low probability (0.47 for "for", 0.54 for "yesterday", 0.56 for "Rs") even though the translation is correct, because there were several correct options ("Rs" or "rupees", "yesterday" at the start or end). Low confidence does not always mean wrong.


## 2) Where the model works and where it fails

a) Overall

- Works well: simple sentences (3/3 ok), numbers (4/4 ok), Tanglish (3 ok, 1 minor), context (2 ok, 1 minor).
- Fails: names (2 wrong, 1 minor out of 3), culture and idioms (3 wrong, 2 minor out of 5), spoken Tamil (1 wrong out of 4).
- No output was visibly broken. All 6 wrong translations are fluent English.
- chrF does not separate right from wrong. Correct sentences scored 28.1 to 100, wrong ones 7.5 to 71.9. "Grandma, where does this bus go?" (wrong) scored 71.9, while "The train leaves at 7:45 a.m." (correct) scored 37.2.

b) Which words went wrong

Names:
- திருவள்ளுவர் became "Thiruvananthapuram" (a city). Should be Thiruvalluvar.
- திருக்குறள் also became "Thiruvananthapuram". Should be Thirukkural.
- திருநெல்வேலி became "Thranelveli". Should be Tirunelveli.
- சுப்பிரமணியம் became "Subramanian". Should be Subramaniam.
- கீர்த்தனா became "Keertana". Should be Keerthana.
- தங்கை (younger sister) became just "sister".

Numbers: all correct (1950, 7:45, 3.5 lakh, two thousand twenty-four). "Lakh" was kept and not turned into "million".

Context:
- ஆறு (six) and ஆற்றில் (in the river) were both translated correctly.
- அவர் (respectful, gender not given) became "He". The model assumed male.

Culture and idioms:
- காக்கா பிடிக்கிறான் (buttering up the boss) became a vulgar phrase.
- தலைக்கு மேல் வேலை (swamped with work) became "I've got work to do".
- கிணற்றுத் தவளை (narrow-minded) became "fountain frog".
- அண்ணா (elder brother) became "Grandma".
- பொங்கல் வைத்தார்கள் (cooked pongal) became "put the Pongal".
- செம mass (really awesome) became "a huge hit".

Spoken Tamil:
- சாப்டியா? (did you eat?) became "What is it, Chaptia?". The word was treated as a name.

c) Small changes in the source

- Negation: அவன் வந்தான் gave "He came." and அவன் வரவில்லை gave "He didn't come." Correct.
- Number: 42 gave "I gave 42 rupees." and 24 gave "I gave you 24 rupees." The number was kept, but "you" was added.
- Gender: அவள் பாடம் படித்தாள் gave "She was studying her lesson." and அவன் பாடம் படித்தான் gave "He was studying." The word "lesson" was dropped for "he".
- Written vs spoken negation: வரவில்லை gave "He didn't come." and வரல gave "He's not coming." Same meaning in Tamil, different tense in English.
- Written vs spoken: நேற்று ... சென்றேன் and நேத்து ... போனேன் both gave "I went to the store yesterday." Correct.
- Written vs spoken "did you eat": சாப்பிட்டாயா? gave "Have you eaten?" but சாப்டியா? gave "What is it, Chaptia?". The spoken form breaks it.
- Removing the question mark: சாப்டியா? gave "What is it, Chaptia?" and சாப்டியா gave "Shaptia".
- Short form of a name: திருவள்ளுவர் gave "Thiruvananthapuram wrote the Thiruvananthapuram." and வள்ளுவர் gave "The villain wrote the crime."


## 3) Fixing one repeated failure: names

Why names: they failed in 2 of 3 name sentences, and many Tamil names are also normal words, so the model translates the meaning instead of keeping the name:
- கன்னியாகுமரி (Kanyakumari) became "Virgo".
- இளங்கோவடிகள் (Ilango Adigal) became "Young men".
- வள்ளுவர் (Valluvar) became "The villain".
- செல்வி (Selvi) became "She".

The fix: before translating, known names from a list are written in English letters inside the Tamil sentence. Tamil endings are kept after a hyphen.

Before: அருண் தன் தங்கை கீர்த்தனாவுடன் மதுரைக்குச் சென்றான்.
After: Arun தன் தங்கை Keerthana-வுடன் Madurai-க்குச் சென்றான்.

This works because part 2 showed the model keeps English words in Tanglish sentences unchanged.

Results on the sentences used to build the fix:
- "...his sister Keertana." became "...his sister Keerthana."
- "Dr. Subramanian lives in Thranelveli." became "Dr. Subramaniam is from Tirunelveli."
- "Thiruvananthapuram wrote the Thiruvananthapuram." became "Thiruvalluvar wrote Thirukkural."
- "The villain wrote the crime." became "Valluvar wrote Thirukkural."


## 4) Did the fix work on new sentences?

Tested on 9 new sentences that were not used while building the fix.

- Names correct went from 8/17 to 16/17.
- Average chrF went from 62.1 to 86.2.

Before and after:
- "Kanaki burned the Madurai." became "Kannagi burned Madurai."
- "Bharathiar was born in Etaipuram." became "Bharathiyar was born in Ettayapuram."
- "She works in Coimbatore." became "Selvi works in Coimbatore."
- "Rajaraja Sojan built a great temple in Thanjavur." became "Rajaraja Chola Thanjavur built a great temple."
- "Young men wrote some poems." became "Ilango Adigal wrote the Silappatikaram."
- "Muthul Lakshmi Reddy is a doctor." became "Muthulakshmi Reddy is a doctor."
- "He went to Velu." became "He went to Velu Thoothukudi."
- "Tomorrow Kavya and Praveen will come to Virgo." became "Kavya and Praveen will come to Kanyakumari tomorrow."
- "Aruna will come tomorrow." became "Arun-ah is coming tomorrow."

Where the fix failed or made it worse:
- தஞ்சாவூர் had no Tamil ending, so after the fix it sat next to another English name and got merged: "Rajaraja Chola Thanjavur".
- Same problem with Velu and Thoothukudi. They were merged, and Velu was lost as the subject.
- அருணா (Aruna) is not in the list, but it starts with அருண் (Arun), so the fix changed it to "Arun-ஆ". It was correct before and wrong after.
- In one build sentence, "lives in" became "is from".

Of the 26 sentences in part 2, only the 3 name sentences changed, all for the better.

Limit: the fix only works for names already in the list.


## 5) Spotting bad translations without the correct answer

Signals used:
1. Confidence: the model's average probability for its own output.
2. Agreement: how similar the model's top 4 candidate translations are.
3. Round trip: translate the English back to Tamil and compare with the original.
4. Numbers: check that every number in the source is in the output.

How each signal did:
- Confidence: correct sentences 0.57 to 0.83 (mean 0.72), wrong ones 0.36 to 0.64 (mean 0.54). This separated them best; they only overlap between 0.57 and 0.64.
- Agreement: correct 22.1 to 85.7 (mean 70.0), wrong 48.5 to 80.7 (mean 64.1). It barely separated them.
- Round trip: correct 6.9 to 100 (mean 54.6), wrong 3.2 to 71.2 (mean 38.0). It is unfair to Tanglish and spoken Tamil, because the back-translation comes out in formal Tamil, so correct Tanglish sentences scored very low (6.9 and 18.6).
- Numbers: never fired, because all numbers were translated correctly.

The round trip does show the mistake to a Tamil reader:
- For "Grandma, where does this bus go?" the back-translation is பாட்டி, இந்த பஸ் எங்கே போகிறது? (பாட்டி means grandma, not அண்ணா).
- For "Thiruvananthapuram wrote the Thiruvananthapuram." the back-translation is திருவனந்தபுரம் திருவனந்தபுரத்தை எழுதினார்.

Flag rule: flag if any signal is below its threshold, or if a number is missing. Thresholds were set from the correct sentences in part 2: confidence 0.68, agreement 61.6, round trip 45.7.

On the part 2 sentences (where the thresholds were set): 8 of 15 correct, 5 of 5 minor and 6 of 6 wrong were flagged.

On the 9 new sentences (without the fix): all 9 were flagged, all because of low confidence.
- 8 of the 9 had a wrong or misspelled name, so all of them were caught.
- 1 was flagged wrongly: "Aruna will come tomorrow." was correct.
- Almost every sentence with names gets low confidence, so the flag mostly detects "this sentence has names", not exactly "this translation is wrong".

When to send a translation for human review:
- confidence is low,
- or the back-translation changes a name or key word,
- or a number is missing,
- or the source has things the model is bad at: names not in the list, idioms, spoken Tamil.
