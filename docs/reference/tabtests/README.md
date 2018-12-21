---
title: Tabtest Reference
version: 0.0.1
---

# Tabtest Reference

Each tabtest are represented as a table, where:

- the first row contains columns with pin labels or [time flow](#time-flow) special column name
- all next rows contains columns with some input and expected output values. See [literals](#literals) section for details.
- each row is one test case that executed within one [transaction](/docs/guide/execution-model/) of XOD program
- BUT if tested node contains some state inside it [will be shared](/docs/guide/testing-patches/#impureness) between all test cases


<a name="literals"></a>
## Literals

Generally, literals for tabtests are the same that are used in the Inspector. But also there are few special literals for tabtests only.

- **Numbers**
  <table class="ui table compact">
    <tbody class="top aligned">
      <tr>
        <td>Integers</td>
        <td><code>23</code>, <code>-76</code></td>
        <td></td>
      </tr>
      <tr>
        <td>Floats</td>
        <td><code>0.035</code>, <code>-29.7054</code></td>
        <td></td>
      </tr>
      <tr>
        <td>NaN</td>
        <td><code>NaN</code></td>
        <td>It's "valid" Number, that can be produced sometimes. For example, by diving zero by zero.</td>
      </tr>
      <tr>
        <td>Infinities</td>
        <td><code>Inf</code>, <code>-Inf</code></td>
        <td>Note, that these Numbers are bigger or smaller even than maximum or minimum possible values of type uint32.</td>
      </tr>
      <tr>
        <td>Approximate floats</td>
        <td><code>0.123~</code>, <code>-384.7583~</code></td>
        <td>Special literals for tabtests and for expected outputs only. Since we're dealing with floats in the XOD some math can give us float numbers like <code>0.49999999999</code> or sometimes the same calculation may give us <code>0.50000000001</code>, but both of them expected as <code>0.5</code>. Alternatively, another one example: <code>-384.75833333</code> can be expected as <code>-384.7583</code>. So when you put a tilde at the end of the number it tells to test runner with desired precision we want to test: <code>0.5~</code> or <code>-384.7583~</code>. All the next numbers are rounded.</td>
      </tr>
    </tbody>
  </table>

- **Strings**

  When we write string literals for `input-string` in the Inspector, it places quotes automatically, but when we deal with generic types, you have to place it manually. So in the tests, you have to write it manually too. If you want to deal with quotes inside your string, escape it with a backslash, like this: `"He said \\"Hello\\" to me"`.

- **Booleans**

  `true` or `false`

- **Pulses**

  `pulse` or `no-pulse`
  This values in the inputs mean that we send a pulse or do not.
  In the outputs — do we expect pulse or not.


<a name="time-flow"></a>
## Time flow column

Name the column: `__time(ms)`

Values can be only positive integers or zero.

If you test something with `defer` nodes inside, do not forget to increment the time on each test case or `defer` node on the execution of the next test case will think that is the same transaction and won't fire the output pulse or output-value.
