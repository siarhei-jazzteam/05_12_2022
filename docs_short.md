### Docstring considerations

- _Overall_: The goal of doctest is to provide documentation, and confirm that
  the documentation works. This is different from unit-testing. So:
  - Keep examples simple.
  - Avoid long or complicated outputs.
  - Use round numbers if possible.
- _Output format_: The output of the snippet needs to be directly beneath the
  code that’s generating the output. Also, the output in the docstring has to
  be exactly equal to what the output would be after the code is executed. See
  the above example. Also, check out
  [this part](https://docs.python.org/3/library/doctest.html#warnings) in the
  DocTest documentation. If the output exceeds the 80 line limit, you can put
  the extra output on the new line and DocTest will recognize it. For example,
  see multi-line blocks below.
- _Globals_: The <code>&#96;tf&#96;</code>, `np` and `os` modules are always
  available in TensorFlow's DocTest.
- _Use symbols_: In DocTest you can directly access symbols defined in the
  same file. To use a symbol that’s not defined in the current file, please
  use TensorFlow’s public API `tf.xxx` instead of `xxx`. As you can see in the
  example below, <code>&#96;random.normal&#96;</code> is accessed via
  <code>&#96;tf.random.normal&#96;</code>. This is because
  <code>&#96;random.normal&#96;</code> is not visible in `NewLayer`.
