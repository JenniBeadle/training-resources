# Module 1A: Why Does Text Break?

**Duration:** 45-60 minutes  
**Level:** Has Knowledge  
**Goal:** Recognize patterns in broken text and understand why display problems occur

---

## Connect (10 minutes)

### The Mystery of the Broken Lexicon

You've just received an email from Maria, a language consultant working with a community in Papua New Guinea. She's excited—the community has completed their first phonetic lexicon with 500 entries, all carefully transcribed in IPA. But there's a problem.

When she opens the file on her computer, it looks like this:

```
[ᵐb̪ɔ̃ː] → [▯b̪▯▯] 
[ʈʂʰɤ] → [▯▯▯▯]
[ɲɟa] → [▯▯a]
```

Half the symbols have turned into boxes. Maria can't send this to the print shop. She can't share it with the community. The work isn't wrong—but it's invisible.

**This happens all the time in language technology work.**

### Your Turn: Recognition Practice

Think about your own experience (or imagine if you're new to this):

- Have you ever opened a document and seen boxes where letters should be?
- Have you received a file where accent marks appeared in the wrong place?
- Have you seen text that looked like scrambled nonsense: "Ã©tÃ©" instead of "été"?

**Take 2 minutes:** If you're working with others, share one example. If you're working alone, jot down what you've seen or what you worry about encountering.

---

## Content (25-30 minutes)

### The Three Layers of Text Display

When you see text on a screen, three things have to work together perfectly:

1. **The encoding** tells the computer which character is which
2. **The font** provides the shapes (glyphs) to draw those characters  
3. **The rendering engine** puts it all together and displays it

When text "breaks," one of these three layers has failed. Your job as a language technologist is to figure out *which one*.

Think of it like a restaurant:
- The **encoding** is the menu (tells you what's available)
- The **font** is the kitchen (has the ingredients to make the dishes)
- The **rendering engine** is the chef (puts it all together)

If the customer orders something not on the menu (encoding problem), or the kitchen doesn't have the ingredients (font problem), or the chef doesn't know the recipe (rendering problem)—the dish doesn't arrive correctly.

### Pattern 1: The Box (□ or ▯)

**What you see:** Boxes, rectangles, or question marks where characters should be

**What it usually means:** The font doesn't have the glyph for that character

**Example:** Maria's IPA symbols are turning into boxes because her default font (probably Arial or Times New Roman) doesn't include IPA characters like [ᵐ] or [ʈ] or [ɲ].

**The key insight:** The computer *knows* which character it's supposed to display (the encoding is working), but the font can't draw it.

### Pattern 2: The Wrong Mark in the Wrong Place

**What you see:** Accent marks floating above or below where they should be, diacritics on the wrong letter, or marks that look "stacked" incorrectly

**Example:**
```
Intended: bɔ̃ː  
Displayed: bɔ ̃ː (with the tilde floating separately)
```

**What it usually means:** This is often an encoding issue—specifically, a problem with how combining characters are stored or how normalization is handled (we'll dig into this in Module 1B).

**The key insight:** The font might be fine, but the way the characters are *encoded* in the file is causing them to display incorrectly.

### Pattern 3: Mojibake (Scrambled Nonsense)

**What you see:** Text that looks like random gibberish, often with strange accented characters where normal letters should be

**Example:**
```
Intended: "Hello"  
Displayed: "H�llo" or "Hèllo" or "æäö▯"
```

**What it usually means:** The file was saved in one encoding (like UTF-8) but opened in another (like Windows-1252), or vice versa. The computer is misinterpreting the data.

**The key insight:** The *data* is fine—it's just being read with the wrong decoder. Like trying to read a French book with a Spanish dictionary.

### Activity: Pattern Recognition (10 minutes)

You're going to look at six broken text samples. For each one, identify which pattern you're seeing:

- **Box** (font missing glyphs)
- **Wrong mark** (encoding/combining character issue)  
- **Mojibake** (encoding mismatch)

**Sample 1:** `The café has good cof▯ee`  
**Sample 2:** `The café has good coffee` (but the é has a floating accent)  
**Sample 3:** `The cafÃ© has good coffee`  
**Sample 4:** `[ŋ] → [▯]`  
**Sample 5:** `naÃ¯ve`  
**Sample 6:** `n̪aː` (where the diacritic under the n is floating)

**Answers:**
1. Box (font doesn't have that glyph)
2. Wrong mark (combining character issue)
3. Mojibake (UTF-8 read as Windows-1252)
4. Box (IPA character missing from font)
5. Mojibake (UTF-8 read as Windows-1252: "naïve")
6. Wrong mark (combining diacritic not attached correctly)

**How did you do?** If you got 4 out of 6, you're building good pattern recognition. If you got fewer, that's fine—this skill develops with practice.

---

## Challenge (10 minutes)

### Your Diagnostic Moment

Let's return to Maria's lexicon. Look at these three entries from her file:

```
Entry 1: [ᵐb̪ɔ̃ː] → displays as [▯b̪▯▯]
Entry 2: [ʈʂʰɤ] → displays as [▯▯▯▯]  
Entry 3: [ɲɟa] → displays as [▯▯a]
```

**Question 1:** Which pattern is this? (Box, Wrong mark, or Mojibake?)

**Question 2:** Which of the three layers (encoding, font, rendering) is most likely the problem?

**Question 3:** What would you tell Maria to try first?

**Take 5 minutes to think through your answers.** If you're working with others, discuss. If you're alone, write down your reasoning.

### Discussion

**Answer 1:** This is the **Box** pattern—characters are turning into boxes/rectangles.

**Answer 2:** Most likely a **font** problem. The computer knows which characters these are (encoding is working), but the font can't draw them.

**Answer 3:** Tell Maria to try changing the font to one that supports IPA characters. SIL has several fonts designed specifically for linguistic work that include comprehensive IPA coverage.

**What you could say to Maria:**  
"The good news is your data isn't corrupted—the computer knows which characters you've typed. The problem is your font doesn't have those shapes. Try switching to a font like Charis SIL or Doulos SIL, which are designed for phonetic transcription. You can download them free from software.sil.org/charis/"

---

## Change (5-10 minutes)

### Building Your Recognition Instinct

You've just learned to recognize three common patterns of broken text. This is your **diagnostic foundation**.

In the next module (1B: The Three Culprits), we'll dig deeper into *why* these patterns happen—how encoding, fonts, and rendering actually work. But for now, you have the most important skill: **pattern recognition**.

### What This Means for Your Work

Next time you encounter broken text:

1. **Don't panic.** Look for the pattern.
2. **Ask yourself:** Is this boxes (font)? Wrong marks (encoding)? Mojibake (encoding mismatch)?
3. **Start your diagnosis** from there.

This single skill—recognizing the pattern—will save you hours of random troubleshooting.

### Before Module 1B

**Quick reference bookmark:** Go to https://software.sil.org/fonts/ and bookmark this page. In Module 1B, we'll explore these fonts in detail, but for now, just know where to find them.

**Optional exploration:** If you have files with IPA or other special characters, open them and look for these patterns. Can you identify which type of problem you're seeing?

### Reflection Question

What's one situation in your current work where this pattern recognition would be helpful? (You don't need to answer this now—just keep it in mind as we move forward.)

---

**End of Module 1A**
