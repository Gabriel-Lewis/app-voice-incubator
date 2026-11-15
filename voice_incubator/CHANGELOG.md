# Changelog

## 0.3.1

- **Updating from 0.2 works.** The language now defaults to English, what
  0.2 spoke. 0.3.0 had no default, and the update failed: the Supervisor
  restarts the app after updating it, and refused to without a language.
  The app was updated all the same, and stopped; picking a language on its
  Configuration tab starts it again.
- **Enrolling a speaker no longer crashes on older x86 CPUs.** Silero VAD
  (pysilero-vad 3.4.0) needed AVX2, so on CPUs without it (many Celeron,
  Atom and Pentium boxes) the app exited with signal 4 as soon as Silero
  loaded: enrolling, and ending a command. pysilero-vad 3.4.1 picks the
  best instructions the CPU has when it loads.
- **Alarms take times as speech-to-text writes them.** "Five fifty" written
  as 5.50, "five oh one" as 5-0-1 and "five seventeen" as 517 set the alarm
  for that time; in a reminder they're times after "at" or "by", or before
  am or pm. "7 a.m." is 7 AM, not 7 PM. Times are said naturally: "5 PM",
  not "five zero zero P M", and ":07" as "oh seven".
- **Home Assistant devices with three-part identifiers don't break the
  connection.** Some integrations register them, which stopped the
  catalog from loading.
- **Free text keeps its last words.** Text that ends in "you" keeps it,
  "announce that I love it" ends on "it", and a "please" after it
  ("announce dinner is ready please") is left out instead of losing the
  command.

## 0.3.0

- **The app's language is picked on its Configuration tab, and it has no
  default.** Updated from 0.2, the app won't start until one is picked.
  German, Spanish, French, Italian, Dutch, Polish, Russian and Ukrainian
  now have English's commands, versions, slots, lists and actions, alarms
  and reminders on the calendar, and two commands said together. The
  language's speech model is downloaded at the first start. Piper's voices
  are the language's, and GLiNER is English only.
- **Wake word training is installed from the Wake words page.** TensorFlow
  is no longer in the image (3.0 GB down to 1.1 GB on amd64), and the
  training kit no longer has to be copied in by hand. Install downloads
  about 1.2 GB on amd64 and 900 MB on aarch64, and takes about 4 GB in the
  data folder. Remove frees it again. Updates keep it, and backups leave it
  out.
- **A training voice that won't download fails the training.** It used to
  wait without saying anything, trying again every 30 s; now the training
  stops after three tries and says why.
- **Wake words are trained with the new libritts_r voice.** It has 904
  speakers where the old one had 2306, with other ids. A saved speaker it
  no longer has speaks with the voice's default.
- **A command said right after the wake word isn't missed.** Silero now
  starts listening on the quiet before the wake word. Started on "okay",
  it scored a plainly said command 0.36, under the 0.5 that starts it,
  and the call ended "no speech was heard".
- **Adding from the library is quick.** The library's sentences are
  embedded in the background after the first build (100 s once on an HA
  Green), so adding 12 commands no longer takes 23 s. The Add buttons say
  "Adding…" until the commands are recognized.
- **The app's logo reads in dark mode too.** The Supervisor shows one logo
  for both themes, and its dark gray wordmark all but disappeared on a dark
  card; it is blue now.

## 0.2.1

- **Wake words are verified on the AEC capture.** The VPE's AGC capture
  sometimes sits about 35 dB down, and near misses ("okay natalie") got
  through on it. The command is still heard on the AGC capture; a call
  without AEC audio is verified as before, and `--wake-capture agc` puts it
  back for every call.
- **The second opinion doesn't overturn a near miss.** A wake word Kaldi
  refused is no longer accepted on the ratio when it sounds more like one of
  37 of "okay nabu"'s confusables ("okay natalie", "okay taboo" with a long
  last vowel) than like it. The call's trace says which.
- **Sentence triggers are known by their sentence.** An automation with no
  description is known to the fallback by its first sentence, each slot by
  its name ("how is the sport game going in city"), not its name: GLiNER
  scored "Game info" 0.27 for "how's the basketball game going in New
  Orleans?", too unsure to run it, and the sentence 0.96. Hints say it the
  same way.

## 0.2.0

- **Voice Labs is Voice Incubator.** As a Home Assistant app it is a new
  one (slug `voice_incubator`): what Voice Labs kept doesn't come along by
  itself. Everything new since Voice Labs is in the source's
  [changelog](https://github.com/OHF-Voice/voice-incubator/blob/v0.2.0/CHANGELOG.md#020).
