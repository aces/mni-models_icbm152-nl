AUTOMAKE_OPTIONS = check-news
ACLOCAL_AMFLAGS = -I local_m4

MNI_MODELS := mni-models

# mess with the data install directory
pkgdatadir = $(prefix)/share/$(MNI_MODELS)

MODEL_BASE := icbm_avg_152
PRIM_MODAL := t1_tal_nlin_symmetric_VI
MODELS := $(MODEL_BASE)_$(PRIM_MODAL)

# datestamp
DATE_STAMP := `date +%D-%T`

# package data
noinst_DATA := \
	$(MODELS:=.mnc.gz) \
	$(MODEL_BASE)_$(PRIM_MODAL)_eye_mask.mnc.gz \
	$(MODEL_BASE)_$(PRIM_MODAL)_mask.mnc.gz \
	$(MODEL_BASE)_$(PRIM_MODAL)_mask.obj.gz \
	$(MODEL_BASE)_$(PRIM_MODAL)_headmask.mnc.gz \
        $(MODEL_BASE)_$(PRIM_MODAL)_vent.mnc.gz \
        $(MODEL_BASE)_$(PRIM_MODAL)_vent.obj.gz \
	Cerebellum_Ventricles_SubCortical_Mask-VI.mnc.gz

# installed data data
pkgdata_DATA := \
	$(MODELS:=.mnc) \
	$(MODELS:=_mask.mnc) \
	$(MODELS:=_mask.obj) \
	$(MODELS:=_eye_mask.mnc) \
	$(MODELS:=_headmask.mnc) \
	$(MODELS:=_vent.mnc) \
	$(MODELS:=_vent.obj) \
	Cerebellum_Ventricles_SubCortical_Mask-VI.mnc

EXTRA_DIST = $(noinst_DATA) $(m4_files) \
	epm-header.in

m4_files = local_m4/ac_prog_gzip.m4 \
	local_m4/ac_prog_make_model.m4

# be tidy
CLEANFILES = $(pkgdata_DATA) $(MODELS:=.mnc)

# gunzip rule
%: %.gz
	$(gzip) -d -c $< > $@

all: $(noinst_DATA:.gz=) $(DATE)
	for i in $(pkgdata_DATA); \
	do \
	  if [ $${i: -4} == ".mnc" ]; then \
            echo "Adding mni-model header to $$i"; \
            minc_modify_header \
			  -sinsert mni-model:package=$(PACKAGE) \
			  -sinsert mni-model:version=$(VERSION) \
			  -sinsert mni-model:compile-date=$(DATE_STAMP) \
			  -sinsert mni-model:compile-user=$$USER \
			  $$i; \
	  fi \
        done
