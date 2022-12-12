# Contribute to the TensorFlow API documentation

## Testable docstrings

TensorFlow uses [DocTest](https://docs.python.org/3/library/doctest.html) to
test code snippets in Python docstrings. The snippet must be executable Python
code. To enable testing, prepend the line with `>>>` (three left-angle
brackets). For example, here's a excerpt from the `tf.concat` function in the
[array_ops.py](https://www.tensorflow.org/code/tensorflow/python/ops/array_ops.py)
source file:

 ```
def concat(values, axis, name="concat"):
  """Concatenates tensors along one dimension.
  ...

  >>> t1 = [[1, 2, 3], [4, 5, 6]]
  >>> t2 = [[7, 8, 9], [10, 11, 12]]
  >>> concat([t1, t2], 0)
  <tf.Tensor: shape=(4, 3), dtype=int32, numpy=
  array([[ 1,  2,  3],
         [ 4,  5,  6],
         [ 7,  8,  9],
         [10, 11, 12]], dtype=int32)>

  <... more description or code snippets ...>

  Args:
    values: A list of `tf.Tensor` objects or a single `tf.Tensor`.
    axis: 0-D `int32` `Tensor`.  Dimension along which to concatenate. Must be
      in the range `[-rank(values), rank(values))`. As in Python, indexing for
      axis is 0-based. Positive axis in the rage of `[0, rank(values))` refers
      to `axis`-th dimension. And negative axis refers to `axis +
      rank(values)`-th dimension.
    name: A name for the operation (optional).

    Returns:
      A `tf.Tensor` resulting from concatenation of the input tensors.
  """

  <code here>
 ```

Note: TensorFlow DocTest uses TensorFlow 2 and Python 3.

### Make the code testable with DocTest

Currently, many docstrings use backticks (```) to identify code. To make the
code testable with DocTest:

- Remove the backticks (```) and use the left-brackets (>>>) in front of each
  line. Use (...) in front of continued lines.
- Add a newline to separate DocTest snippets from Markdown text to
  render properly on tensorflow.org.

### Customizations

TensorFlow uses a few customizations to the builtin doctest logic:

- It does not compare float values as text: Float values are extracted from
  the text and compared using `allclose` with _liberal `atol` and `rtol`
  tolerences_. This allows :
  - Clearer docs - Authors don't need to include all decimal places.
  - More robust tests - Numerical changes in the underlying implementation
    should never cause a doctest to fail.
- It only checks the output if the author includes output for a line. This
  allows for clearer docs because authors usually don't need to capture
  irrelevant intermediate values to prevent them from being printed.

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
  - _Globals_: The, `np` and `os` modules are always
  available in TensorFlow's DocTest.
  <```>&#96;tf.random.normal&#96;</```>. This is because
