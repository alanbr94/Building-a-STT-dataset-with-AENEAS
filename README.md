# Building-a-STT-dataset-with-AENEAS
A tutorial based on @klintch to build or increase a STT (Deepspeech) dataset. 

This tutorial is based on "https://medium.com/@klintcho/creating-an-open-speech-recognition-dataset-for-almost-any-language-c532fb2bc0cf", who did an excelent work. Because of his tutorial, I was able to create a 20 hours of transcription+audio for deepspeech. So here I'll add some tips that I learned from that process.
The goal here is not to be the main tutorial for Aeneas force alignment, but a method to help you with some tips.

### **FIRST OF ALL, How to install Aeneas?**

Aeneas automatically generate a synchronization map between a list of text fragments and an audio file containing the narration of the text.
You can find more information about Aeneas in: https://pypi.org/project/aeneas/1.4.0.0/ 
If you are using windows, you have to use Python 2.7. On the other hand, you can use Google Colab. For installation in python2.7, the Aeneas website has a good tutorial, so I will explain how to install on Colab. Note that all of these commands are from Linux, in Colab you have to add "!" before the command if it is from Linux.

Execute the following commands:
```
!wget https://raw.githubusercontent.com/readbeyond/aeneas/master/install_dependencies.sh
!bash install_dependencies.sh
!pip install ffprobe
!pip install ffmpeg
!git clone https://github.com/ReadBeyond/aeneas.git
%cd /content/aeneas
!pip install -r requirements.txt
!python setup.py build_ext --inplace
!python aeneas_check_setup.py
```

Done!

From here, you can follow @klintch tutorial. Just a tip, it will work better if you just put every sentence in a line. For example, if you find a "," in the text, you can skip a line. Another tip is, don't let the sentences too much short, like "Oh!" or "No", it will make harder to do the fine tuning step, try to let at least two words in a sentence.
At the end of this process, you'll have a syncmap.

### **During the fine tuning**

Clone or download https://github.com/ozdefir/finetuneas, open the .html. Load your syncmap(.json) and your audio.

You'll find something like that:

![image](https://user-images.githubusercontent.com/71395951/138873892-b1b25b4b-d463-45b2-ba5d-b0b3d9cfeb40.png)

You have to push the button ![image](https://i.imgur.com/dJwiy8h.jpeg) or ![image](https://i.imgur.com/CNK2Pyo.jpeg) in order to determine the right duration of your audio segment.

Note that usually the firsts sentences are better synchronized, while from the middle to the end of the syncmap, the sentences are a little bit unsynchronized.

Here is another tip. In order to make the process faster, just look at the end of the audio segments, and try to synchronize it the most accurate you can do. For eg:

Here are three sentences: 

> "I belong to Bailum & Barney's Great Consolidated Shows--three rings in one tent and a menagerie on the side."(I) 

> "But we're only three days out of Ashley - at the most.."(II) 

> "We'd have waited for you, but we was outnumbered three to one and didn't have time to look for you."(III) 

On the first sentence, rather than looking at the beginning and the ending, just look at the ending. 

These are three sentences of someone talking, so "...menagerie on the sind" is followed by "But we're only...". 

If you cut the ending of the first sentence exactly when "side" finishes, the next sentence will already starts where the "But" starts, and you don't have to worry about the second sentence's beginning. 

The same for the third sentence. If you synchronized the end of the second sentence exactly where "most.." finishes, the "We'd have..." will start at the right time.

Then you look just at the end of the third sentence for the fourth, and so on. It will save a lot of your time.

Finish your force alignment by the generate of .csv step. 
