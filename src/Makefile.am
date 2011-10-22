bin_PROGRAMS = mapmap

AM_CXXFLAGS = \
	$(BOOST_CPPFLAGS) \
	$(CLUTTER_CFLAGS) \
	$(CLUTTERGST_CFLAGS) \
	$(GLIB_CFLAGS) \
	$(LIBLO_CFLAGS) \
	-I$(top_srcdir)

LDADD = \
	$(BOOST_PROGRAM_OPTIONS_LIB) \
	$(BOOST_LIBS) \
	$(CLUTTER_LIBS) \
	$(CLUTTERGST_LIBS) \
	$(GLIB_LIBS) \
	$(LIBLO_LIBS)

mapmap_CXXFLAGS = $(AM_CXXFLAGS)
mapmap_LDADD = $(LDADD)

headers = 

mapmap_SOURCES = \
	main.cpp \
	$(headers)

