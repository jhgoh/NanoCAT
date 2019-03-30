# TZWi
Yet another CMS NanoAOD tools focusing on specific analyses.
  * Management area: https://app.asana.com/0/1115311953973868/list

We'd like to cover:

  * ttbar dilepton analysis
    * inclusive cross section, differential cross section
    * ttbar+bbbar cross section ratio to ttbar+jets
  * ttbar rare decays
    * t->qZ, ttbar and single top

**See also**
- https://twiki.cern.ch/twiki/bin/view/CMSPublic/WorkBookNanoAOD
- NanoAOD dataformat descriptions
  - 2016 samples: https://cms-nanoaod-integration.web.cern.ch/integration/master-102X/mc80X_doc.html
  - 2017 samples: https://cms-nanoaod-integration.web.cern.ch/integration/master-102X/mc94Xv2_doc.html
  - 2018 samples: https://cms-nanoaod-integration.web.cern.ch/integration/master-102X/mc102X_doc.html

## Installation
```
cmsrel CMSSW_10_2_11
cd CMSSW_10_2_11/src
cmsenv
git-cms-init
git cms-merge-topic cms-nanoAOD:master-102X
git checkout -b nanoAOD cms-nanoAOD/master-102X
git clone https://github.com/cms-nanoAOD/nanoAOD-tools.git PhysicsTools/NanoAODTools
git clone https://github.com/cms-kr/TZWi
scram b -j
```

## (Optional) Customized NanoAOD production
```
cd TZWi/NanoAODProduction/test
./generateConfig.sh ## This will produce 3 sets of 3 cfg files...
crab submit...
```

## List up NanoAOD samples
Update sample list, produce file lists
```
tzwi-udpatedataset $CMSSW_BASE/src/TZWi/NanoAODProduction/data/datasets/NanoAOD/201*/*.yaml
```

## Run postprocessors
```
cd $CMSSW_BASE/src/TZWi/TopAnalysis/test/ttbarDoubleLepton
for MODE in MuMu MuEl ElEl; do
    for F in `find NanoAOD/2017/ -name '*.txt'`; do
        N=`cat $F | wc -l`
        for I in `seq $N`; do
            echo $MODE $F 1 $((I-1));
        done
    done
done | xargs -l1 -P20 ./01_prod_ntuple.sh > /dev/null
```

## Make histograms
```
./02_make_histograms.sh
```
