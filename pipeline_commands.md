# Chandra ACIS-S Spectral Analysis Pipeline: WD 2226−210 (ObsID 631)
This log documents the data reduction via CIAO 4.18 and spectral modeling inside XSPEC 13.0 for Project-10.

## Part 1: CIAO Data Reduction & Extraction (Run in Bash Shell)

```bash
# B.1 Retrieval and reprocessing
download_chandra_obsid 631
chandra_repro indir=631 outdir=631/repro

# B.2 Flare screening
cd 631/repro
dmextract "acisf00631_repro_evt2.fits[ccd_id=7,energy=5000:10000][bin time=::259.28]" bkg_lc.fits opt=ltc1 clobber=yes
dmstat "bkg_lc.fits[cols count_rate]"
dmlist "bkg_lc.fits[cols time,count_rate]" data
dmgti bkg_lc.fits bkg_gti.fits userlimit="count_rate<0.78" clobber=yes
dmcopy "acisf00631_repro_evt2.fits[@bkg_gti.fits]" clean_evt2.fits clobber=yes
dmkeypar clean_evt2.fits EXPOSURE echo+

# B.3 Position verification (Target centroid sky coordinates)
dmstat "clean_evt2.fits[energy=300:2000][sky=region(src.reg)][cols energy]"
dmcoords clean_evt2.fits asolfile=pcadf00631_000N001_asol1.fits option=sky x=4092.8634 y=4143.1983 celfmt=hms
pget dmcoords ra dec

# B.4 Spectral extraction (Grouping with minimum 15 counts per bin)
specextract "clean_evt2.fits[sky=region(src.reg)]" outroot=wd2226 bkgfile="clean_evt2.fits[sky=region(bkg.reg)]" grouptype=NUM_CTS binspec=15 clobber=yes

# Verify automatic header keyword linking
dmkeypar wd2226_grp.pi BACKFILE echo+
dmkeypar wd2226_grp.pi RESPFILE echo+
dmkeypar wd2226_grp.pi ANCRFILE echo+

# Sensitivity variantExtractions (Alternative test groupings)
specextract "clean_evt2.fits[sky=region(src.reg)]" outroot=wd2226_g20 bkgfile="clean_evt2.fits[sky=region(bkg.reg)]" grouptype=NUM_CTS binspec=20 clobber=yes
specextract "clean_evt2.fits[sky=region(src.reg)]" outroot=wd2226_g25 bkgfile="clean_evt2.fits[sky=region(bkg.reg)]" grouptype=NUM_CTS binspec=25 clobber=yes
specextract "clean_evt2.fits[sky=region(src.reg)]" outroot=wd2226_g1 bkgfile="clean_evt2.fits[sky=region(bkg.reg)]" grouptype=NUM_CTS binspec=1 clobber=yes
```

## Part 2: XSPEC Fitting Sequence (Run Interactively inside XSPEC Environment)

```tcl
# B.5 Baseline Spectral Fit Setup
log wd2226_fit.log
abund angr
data wd2226_grp.pi
ignore bad
ignore **-0.3 2.0-**
model tbabs*apec
0.016
0.9
1.0
0.0
4.3e-5
fit
error 1 2 5
flux 0.3 2.0 err 1000 90

# Unabsorbed flux generation sequence (No refit parameter check)
newpar 1 0
flux 0.3 2.0
newpar 1 0.0158
fit

# B.6 Systematic Error Testing 
# Test Variant A: Galactic Fixed NH
newpar 1 0.031
freeze 1
fit
error 2 5
thaw 1

# Test Variant B: Cash-Statistic Fitting for Low-Count bins
data wd2226_g1_grp.pi
statistic cstat
ignore bad
ignore **-0.3 2.0-**
fit
error 1 2 5
statistic chi

# Test Variant C: Variable abundance fits & alternative cross-sections
thaw 3
fit
error 3
newpar 3 1
freeze 3
fit
abund wilm
fit

# B.7 Flux Density Extraction (Narrowband 1 keV calculations)
flux 0.99 1.01
newpar 1 0
flux 0.99 1.01
newpar 1 0.0158
fit

# Cflux convolution fitting for explicit error constraints
model tbabs*cflux(apec)
0.0158
0.3
2.0
-12.92
0.896
1.0
0.0
1
freeze 8
fit
error 1 4 5

# B.8 Plot Matrix and Housekeeping Export
setplot energy
cpd spectrum.ps/cps
plot ldata delchi
cpd none
iplot ldata delchi
wdata wd2226_plot
exit
log none
punlearn ardlib
```
