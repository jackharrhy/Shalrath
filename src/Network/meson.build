qt6_dep = dependency('qt6', modules: ['Core', 'Gui', 'Widgets', 'Network'], required: true).as_system()
qt6_rcc = find_program('rcc', required: true)

# Custom function to find moc
moc_paths = [
  '/usr/lib/qt6/libexec/moc',
  'moc',  # This will find moc in the system PATH
]

qt6_moc = []
foreach moc_path : moc_paths
  moc_candidate = find_program(moc_path, required: false)
  if moc_candidate.found()
    qt6_moc = moc_candidate
    break
  endif
endforeach

if qt6_moc.found() == false
  error('Qt 6 moc not found')
endif

moc_files = [
  'NetworkManager.h',
]

moc_outputs = []
foreach moc_file : moc_files
  moc_basename = meson.project_name() + '_' + moc_file.split('/')[-1]
  moc_output = custom_target('moc_' + moc_basename,
    input: moc_file,
    output: 'moc_' + moc_basename + '.cpp',
    command: [qt6_moc, '@INPUT@', '-o', '@OUTPUT@']
  )
  moc_outputs += moc_output
endforeach


network_sources = [
  'NetworkManager.cpp',
] + moc_outputs

network_dep = declare_dependency(
  sources: network_sources,
  include_directories: include_directories('.'),
  dependencies: qt6_dep
)