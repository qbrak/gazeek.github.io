---
## the default layout is 'page'
layout: post
icon: fas fa-at
order: 4
toc: true
---

I wanted a place to put all the links that I care about :).


Open source books (a LOT of them): <https://www.intechopen.com/about-intechopen>

## Math
* Terrence Tao's notes from his blog, he derives a Fourier Transform and talks about Lebesgue measures: <https://terrytao.files.wordpress.com/2010/02/epsilon.pdf>
* [Baumeister's](https://www.math.uni-frankfurt.de/~baumeist/) book on fixed points, metric projections, minimization (non-linear?) <https://www.math.uni-frankfurt.de/~baumeist/Nonex-Kap1.pdf> (manually need to change *.pdf to 1,2,3,4,5,7,8)
* Book on convex optimization from Stanford: <https://web.stanford.edu/~boyd/cvxbook/>
* [Probability Theory: the logic of science](http://www.med.mcgill.ca/epidemiology/hanley/bios601/GaussianModel/JaynesProbabilityTheory.pdf)
* [Linear algebra done right](https://linear.axler.net/)
* [Weak convergence (UTexas)](https://web.ma.utexas.edu/users/gordanz/notes/weak.pdf)
* [Measure theory (Borell, Gotenburg) (e.g. complex measure)](https://www.math.chalmers.se/Math/Grundutb/GU/MAF440/H05/MeasureTheory.pdf)

## Transforms/Complex analysis
* Cool Rice university course (with recorded lectures) on Mathematical Methods from the Physics department:
<https://www.ruf.rice.edu/~baring/phys516/phys516_record.html>

## PCA
* Tutorial <https://arxiv.org/pdf/1404.1100.pdf>

## Stats
* Great website with lots of stats stuff: <https://www.statlect.com/>
* Variance estimate of normal distribuition: <https://www.statlect.com/fundamentals-of-statistics/variance-estimation#:~:text=Variance%20estimation%20is%20a%20statistical,estimator%20of%20the%20population%20variance>.
* Stdev estimate of stdev: <https://stats.stackexchange.com/a/28567>
* Really cool probability course book: <https://www.probabilitycourse.com/preface.php>

## DSP
Really cool book about DSP: <https://www.dspguide.com/>

Here is an article about random sampling: <https://arxiv.org/pdf/1204.0839.pdf>

Here is an article about time-interleaved sampling: <https://www.analog.com/en/analog-dialogue/articles/interleaving-adcs.html#:~:text=Time%20interleaving%20is%20a%20technique,of%20each%20individual%20data%20converter>

Correcting the distortion caused by window function: <https://community.sw.siemens.com/s/article/window-correction-factors>
(however, coherent gain (a.k.a. DC gain) assumes that sum(win)!=1, if sum(win)=1, then coherent gain=1).

Zeropadding vs repeating signal to get more resolution: <https://www.penwatch.net/cms/fft_sampling/>

Cool page about windows and their figures of merit: <https://www.recordingblogs.com/wiki/processing-gain>

## Wifi DSP

Book about wireless DSP processing: <https://www.rle.mit.edu/sia/books/wireless-communications-signal-processing-perspectives/>

Survey of wifi tech (and required theory): <https://www.people.vcu.edu/~ebulut/COMST22_WiFi_Sensing_Survey.pdf>

Related to [CSI](https://en.wikipedia.org/wiki/Channel_state_information): [article](https://7fa7a724-a-62cb3a1a-s-sites.googlegroups.com/site/mdanishnisar/pubs/21_Robust_Channel_Est_Nisar_TSP_2010.pdf?attachauth=ANoY7cqzw5YcZj4oBRZHWPD6SUu4ZdjKmfpv89ch1vTEkTKc1ppFAWs690GM-ws7WSjUo5M2kyQJPA1pe-yt2sx8_lkuc7nQXIeDwZZbeFsUGtUQ2y3B5MWpv5IcyUwqwnHZYEY20SUSrm-5olD0-1Ijqqq1U9jvg1_qQ_T3x8--52-WxLwTF3f455xIF4fgzQVdO4vpndarl3kDYdATPbkznh23BKVk50VE9Rnj_nI2p-Vc2BwUeQT8mfs4bvvUVukpYrGAzFUi&attredirects=0)

### Wavelets
* A guide about wavelet tranform (has 4 parts): <https://ccrma.stanford.edu/~unjung/mylec/WTpart1.html>

### Random processes
* Deriving frequency response of a instantiation of a random process passed through a window: <https://radarsp.weebly.com/uploads/2/1/4/7/21471216/dft_of_noise.pdf>
* How to interpret power density spectra of random processes: <https://www.sgu.ru/sites/default/files/textdocsfiles/2014/10/29/random_proc_slides_5_psd_2pages.pdf>
* Lecture on different methods of estimating power density spectra of random process (periodogram, Bartlett and Welch's methods): <http://www.laurent-duval.eu/Documents-Common/Schuster_G_2010_lect_spectrum_upbw.pdf>
* Standford lectures on DSP, in particular section on pink noise: <https://ccrma.stanford.edu/~jos/sasp/Example_Pink_Noise_Analysis.html>
* Cyclostationary process from arxiv: <https://arxiv.org/pdf/1505.05586.pdf>
  - They literally discuss how to reason about demodulation and filtering with a window in their two exmplaes.

Some notes on estimating power spectrum density for random process:

We could potentially estimate the autocorrelation function (e.g. through averaging) and then FFT that and get the PSD for the random process, however, it seems that direct calculations of periodograms and averaging them is the preferred method from a calculation standpoint.

## Electronics

* This looks like a really cool electronics book: <https://ultimateelectronicsbook.com/#introduction>
* Impedances, Thevenin can be used not only to model electrical systems: <http://web.mit.edu/2.151/www/Handouts/Impedances.pdf>
* Sigme-delta non-idealities analysis: <https://bmas.designers-guide.org/2002/papers/bmas02-koe.pdf>
* Sigma-delta equations and choice of order: <https://www.ieee.li/pdf/essay/calculating_delta-sigma_snr.pdf>

### Electronics noise

* Here is noise model for sampling on a capacitor: <https://www.ttcenter.ir/ArticleFiles/ENARTICLE/3740.pdf>
* Here is a book about designing for low-noise circuits: <https://www.pearl-hifi.com/06_Lit_Archive/14_Books_Tech_Papers/Motchenbacher_Connelly/Low-noise_Electronic_Design.pdf>
* Here is an article about flicker (1/f) noise: <http://www.schmid-werren.ch/hanspeter/publications/2008crcbook.pdf>
