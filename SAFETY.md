# Safety Parameter File Format

The safety parameters are stored in an XML file with a `.xml` extension; the root element should be `safety` or `group_safety`.  If the element is `safety`, all values should have a single value; if the element is `group_safety`, each element is treated as a string that is a whitespace separated list of acceptable values.  In the case of `group_safety`, each list in the children elements should be the same lenght.

Implementation note: When parsing strings that are "enum" values (e.g., the mstop strategy element), implementations should support reading these values in without regard to case (e.g., a value of "Stop" or "stop" should both be interpreted correctly). In the case of non-matching case, a warning should be provided by the API when importing the file. If implementations support writing safety XML files, the case shown in this document should be written.

Each element that can be a child of the root element is described below.

## `<mstop>`

The `<mstop>` element has no attributes.  Its acceptable children are:
- `<strategy>`

## `<strategy>`

The `<strategy>` element (as a child of `<mstop>`) describes the motion stop strategy for this actuator.  Acceptable values are:
- disabled
- off
- hold

For `<safety>` root elements, this is treated as an enumeration; for `<group_safety>`, this is a whitespace-separate list of these values.

## `<position>`

The `<position>` element has no attributes.  Its acceptable children are:
- `<min_limit>`
- `<max_limit>`
- `<limit_strategy>`

(`<min_limit>` and `<max_limit>` elements are shared among `<position>`, `<velocity>`, and `<effort>` elements and are described after these parent elements below)

## `<limit_strategy>`

The `<limit_strategy>` element (as a child of `<position>`) describes the position limit strategy for this actuator.  Acceptable values are:
- disabled
- off
- hold
- spring

For `<safety>` root elements, this is treated as an enumeration; for `<group_safety>`, this is a whitespace-separate list of these values.

## `<velocity>`

The `<velocity>` element has no attributes.  Its acceptable children are:
- `<min_limit>`
- `<max_limit>`

(`<min_limit>` and `<max_limit>` elements are shared among `<position>`, `<velocity>`, and `<effort>` elements and are described after these parent elements below)

## `<effort>`

The `<effort>` element has no attributes.  Its acceptable children are:
- `<min_limit>`
- `<max_limit>`

(`<min_limit>` and `<max_limit>` elements are shared among `<position>`, `<velocity>`, and `<effort>` elements and are described after these parent elements below)

### `<min_limit>` and `<max_limit>`

These elements describe the minimum and maximum values of the corresponding limits, and are floating point values with the special values of `-inf` and `inf` permitted.  (Note that `nan` is not permitted)

For `<safety>` root elements, this is treated as a single floating point number; for `<group_safety>`, this is a whitespace-separate list of these values.

## Example

An example file for a 4-DoF system is shown below:

```
<?xml version="1.0" encoding="UTF-8"?>
<group_safety version="1.0.0">
  <mstop>
    <strategy>off hold disabled off</strategy> 
  </mstop>
  <position>
    <limit_strategy>hold off disabled spring</limit_strategy>
    <min_limit>-1.5 0 -inf -inf</min_limit> 
    <max_limit>1.5 1 inf inf</max_limit>
  </position>
  <velocity>
    <min_limit>-1 -1 -1 -1</min_limit>
    <max_limit>1 1 1 1</max_limit>
  </velocity>
  <effort>
    <min_limit>-1 -1 -1 -1</min_limit>
    <max_limit>1 1 1 1</max_limit>
  </effort>
</group_safety>
```
