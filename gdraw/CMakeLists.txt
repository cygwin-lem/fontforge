# Distributed under the original FontForge BSD 3-clause license

# not included
#  gdrawwacomdriver.c genkeysym.c growcol.c

list(APPEND gdraw_objects_list
  choosericons.c
  colorP.h
  ctlvalues.c
  div_tables.c
  drawboxborder.c
  fontP.h
  gaskdlg.c
  gbuttons.c
  gchardlg.c
  gcolor.c
  gcontainer.c
  gdrawable.c
  gdrawbuildchars.c
  gdraw.c
  gdrawerror.c
  gdrawgimage.c
  gdrawP.h
  gdrawtxt.c
  gdrawtxtinit.c
  gfilechooser.c
  gfiledlg.c
  ggadgetP.h
  ggadgets.c
  ggdkcdraw.c
  ggdkdraw.c
  ggdkdrawlogger.c
  ggdkdrawP.h
  ggroupbox.c
  ghvbox.c
  gimageclut.c
  gimagecvt.c
  gimagexdraw.c
  gkeysym.c
  glist.c
  gmatrixedit.c
  gmenu.c
  gprogress.c
  gradio.c
  gresedit.c
  gresource.c
  gresourceimage.c
  gresourceP.h
  gsavefiledlg.c
  gscrollbar.c
  gspacer.c
  gtabset.c
  gtextfield.c
  gtextinfo.c
  gwidgetP.h
  gwidgets.c
  gxcdraw.c
  gxcdrawP.h
  gxdraw.c
  gxdrawP.h
  hotkeys.c
  xkeysyms_unicode.c
)

if(BUILD_SHARED_LIBS_GDRAW)
  add_library(gdraw ${gdraw_objects_list})
else()
  add_library(gdraw STATIC ${gdraw_objects_list})
endif()

if ( NOT USE_PYTHON_SUBNAME STREQUAL "" )
  set_target_properties(gdraw PROPERTIES OUTPUT_NAME "gdraw-${USE_PYTHON_SUBNAME}")
endif()

if(BUILD_SHARED_LIBS_GDRAW)
  if(WIN32 OR CYGWIN)
    target_link_options(gdraw PRIVATE -Wl,--export-all-symbols) # FIXME
  endif()
  target_link_libraries(gdraw PUBLIC fontforge)
else()
  target_link_libraries(gdraw PRIVATE fontforge)
endif()

if(ENABLE_X11)
  target_link_libraries(gdraw PUBLIC
    X11::X11
    X11::Xi
    X11::Xkb
    Cairo::Cairo
    Pango::Pango
  )
else()
  if(APPLE)
    target_sources(gdraw PRIVATE ggdkcocoa.m)
  endif()
  target_link_libraries(gdraw PUBLIC GDK3::GDK3)
endif()

if(BUILD_SHARED_LIBS_GDRAW)
  if(WIN32 OR CYGWIN)
    install(TARGETS gdraw RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR} LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})
  else()
    install(TARGETS gdraw RUNTIME DESTINATION ${CMAKE_INSTALL_LIBDIR} LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})
  endif()
endif()
