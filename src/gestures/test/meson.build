doctest = dependency('doctest', required: get_option('tests'))
if doctest.found()
  doctest_compile = doctest.partial_dependency(
    compile_args: true,
    includes: true,
  )

  test_exe = executable('test-gestures',
    'MockGestureManager.cpp',
    'test.cpp',
    link_with: gestures,
    dependencies: [
      wftouch,
      doctest_compile
    ]
  )

  test('test gestures', test_exe)
endif
