# assert断言
- 提供了一组用于验证不变量的断言函数。
- https://nodejs.cn/api/assert.html
## assert.AssertionError
```
// 表示断言失败。所有由`node：assert`模块抛出的错误都将是`AssertionError`类的实例。
class AssertionError extends Error {
    // 实际参数值
    actual: unknown;
    // 期望参数值
    expected: unknown;
    // 设置为传入的运算符值。
    operator: string;
    // 指示消息是否自动生成（“true”）。
    generatedMessage: boolean;
    // 值总是`ERR_ASSERTION`，以表明错误是断言错误。
    code: "ERR_ASSERTION";
    constructor(options?: {
        /** 如果提供，则错误消息设置为该值。 */
        message?: string | undefined;
        /** 错误实例上的“actual”属性。 */
        actual?: unknown | undefined;
        /** 错误实例上的“expected”属性。 */
        expected?: unknown | undefined;
        /** 错误实例上的“operator”属性。 */
        operator?: string | undefined;
        /** 如果提供，生成的堆栈跟踪将省略此函数之前的帧。 */
        stackStartFn?: Function | undefined;
    });
}
```
## assert.CallTracker（已弃用）
-  此功能【已弃用】，并将在未来版本中删除。 请考虑使用替代方法，例如`mock` helper函数。
```
class CallTracker {
    /**
     * 包装器函数应该被精确地调用`exact`次。
     * 如果在调用`tracker.verify()`的时候，函数没有被精确地调用，那么`tracker.verify()`将抛出一个错误。
     * 【包装器函数，用于记录调用信息】
     * @param [fn='A no-op function'] 函数
     * @param [exact=1] 次数，默认为1
     * @return 包装的`fn`.
     */
    calls(exact?: number): () => void;
    calls<Func extends (...args: any[]) => any>(fn?: Func, exact?: number): Func;
    /**
     * 【获取包装器函数调用的记录】
     * @param fn 包装的`fn`.
     * @return 一个数组，其中包含对跟踪函数的所有调用。
     */
    getCalls(fn: Function): CallTrackerCall[];
    /**
     * 这些数组包含有关未被调用预期次数的函数的预期调用次数和实际调用次数的信息。
     * 【获取包装器函数实际调用次数和期望调用次数的信息】
     * @return 一个对象的数组，包含有关由`calls`返回的包装函数的信息。
     */
    report(): CallTrackerReportInformation[];
    /**
     * 重置呼叫跟踪器的呼叫。
     * 如果一个被跟踪的函数作为参数传递，调用将被重置。
     * 如果没有参数传递，所有跟踪的函数将被重置。
     * 【重置包装器的调用次数】
     * @param fn 被重置的包装器函数
     */
    reset(fn?: Function): void;
    /**
     * 遍历传递给`tracker.calls（）`的函数列表，并将对未被调用预期次数的函数抛出错误。
     * 【校验调用次数是否ok】
     */
    verify(): void;
}
interface CallTrackerCall {
    thisArg: object;
    arguments: unknown[];
}
interface CallTrackerReportInformation {
    message: string;
    /** 函数被调用的实际次数。 */
    actual: number;
    /** 预期调用函数的次数。 */
    expected: number;
    /** 被包装的函数的名称。 */
    operator: string;
    /** 函数的堆栈跟踪。 */
    stack: object;
}
```
- 示例1：构造包装器函数
```javascript
import assert from 'node:assert';
const tracker = new assert.CallTracker();// 创建呼叫跟踪器
function func() {}
// 返回一个包装func()的函数，该函数必须在tracker.verify()之前精确调用。
const callsfunc = tracker.calls(func);
```
- 示例2：获取包装器函数调用的记录
```js
import assert from 'node:assert';
const tracker = new assert.CallTracker();
function func() {}
const callsfunc = tracker.calls(func);
callsfunc(1, 2, 3);
assert.deepStrictEqual(tracker.getCalls(callsfunc), [{ thisArg: undefined, arguments: [1, 2, 3] }]);
```
- 示例3：获取包装器函数实际调用次数和期望调用次数的信息
```js
import assert from 'node:assert';
const tracker = new assert.CallTracker();// 创建呼叫跟踪器。
function func() {}
// 返回一个包装func()的函数，该函数必须在tracker.verify()之前精确调用。
const callsfunc = tracker.calls(func, 2);
// 返回一个包含callsfunc()信息的数组
console.log(tracker.report());
// [
//  {
//    message: '应该执行func函数2次，但执行了0次。',
//    actual: 0,
//    expected: 2,
//    operator: 'func',
//    stack: stack trace
//  }
// ]
```
- 示例4：重置包装器的调用次数
```js
import assert from 'node:assert';
const tracker = new assert.CallTracker();
function func() {}
const callsfunc = tracker.calls(func);
callsfunc();
assert.strictEqual(tracker.getCalls(callsfunc).length, 1);// 调用一次
tracker.reset(callsfunc);
assert.strictEqual(tracker.getCalls(callsfunc).length, 0);// 调用零次
```
- 示例5：校验调用次数是否ok
```js
import assert from 'node:assert';
const tracker = new assert.CallTracker();// 创建
function func() {}
const callsfunc = tracker.calls(func, 2);// 返回一个包装func（）的函数，该函数必须在tracker.verify（）之前精确调用。
callsfunc();
tracker.verify();// 将抛出错误，因为callsfunc（）只被调用了一次。
```
## assert
```
type AssertPredicate = RegExp | (new() => object) | ((thrown: unknown) => boolean) | object | Error;
namespace assert {
    /**
     * 【抛异常】
     * 抛出带有提供的错误消息或默认错误消息的`AssertionError`。
     * 如果`message`参数是`Error`的实例，那么它将被抛出，而不是`AssertionError`。
     * @param [message='Failed']
     */
    function fail(message?: string | Error): never;
    /**
     * 【值为真】
     * 测试“值”是否为真。它等价于`assert.equal（value，true，message）`.
     * 如果`value`不是true，则抛出`AssertionError`，并将`message`属性设置为等于`message`参数的值。
     * 如果`message`参数为`undefined`，则分配默认错误消息。
     * 如果`message`参数是`Error`的实例，那么它将被抛出，而不是`AssertionError`。
     * 如果没有参数传入，则`message`将被设置为字符串：`No value argument passed to `assert.ok（）`。
     */
    function ok(value: unknown, message?: string | Error): asserts value;
    /**
     * 【相等】
     * 测试`actual`和`expected`参数之间的浅层相等，使用 [`==` 号](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality). 
     * 如果两边都是“NaN”，则对“NaN”进行特殊处理并将其视为相同
     * 如果值不相等，则抛出`AssertionError`，并将`message`属性设置为等于`message`参数的值。
     * 如果“message”参数未定义，则分配默认错误消息。
     * 如果`message`参数是`Error`的实例，那么它将被抛出，而不是`AssertionError`。
     */
    function equal(actual: unknown, expected: unknown, message?: string | Error): void;
    /**
     * 【严格相等性】
     * 测试“actual”和“expected”参数之间的严格相等性，如下所示：
     * 确定[`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).
     * 如果两个值不严格相等，则抛出一个`AssertionError`，并将`message`属性设置为等于`message`参数的值。
     * 如果“message”参数未定义，则分配默认错误消息。
     * 如果`message`参数是`Error`的实例，那么它将被抛出，而不是`AssertionError`。
     */
    function strictEqual<T>(actual: unknown, expected: T, message?: string | Error): asserts actual is T;
    /**
     * 【不相等】
     * 浅层不平等 [`!=` 号](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Inequality). 
     * 如果两边都是“NaN”，则对“NaN”进行特殊处理并将其视为相同。
     * 如果两个值相等，则抛出一个`AssertionError`，并将`message`属性设置为等于`message`参数的值。
     * 如果“message”参数未定义，则分配默认错误消息。
     * 如果`message`参数是`Error`的实例，那么它将被抛出，而不是`AssertionError`。
     */
    function notEqual(actual: unknown, expected: unknown, message?: string | Error): void;
     /**
     * 【严格不平等】
     * 测试“实际”和“预期”参数之间的严格不平等，
     * 确定 [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).
     * 如果两个值严格相等，则抛出一个`AssertionError`，并将`message`属性设置为等于`message`参数的值。
     * 如果“message”参数未定义，则分配默认错误消息。
     * 如果`message`参数是`Error`的实例，那么它将被抛出，而不是`AssertionError`。
     */
     function notStrictEqual(actual: unknown, expected: unknown, message?: string | Error): void;
    /**
     * 【深度相等】
     * 测试`actual`和`expected`参数之间的深度相等。
     * 请考虑使用{@link deepaltEqual}。{@link deepEqual}可能会有令人惊讶的结果。
     * “深度”相等意味着子对象的可验证的“自己”属性也由以下规则递归计算。
     */
    function deepEqual(actual: unknown, expected: unknown, message?: string | Error): void;
    /**
     * 【深度相等】
     * 测试`actual`和`expected`参数之间的深度相等。
     * “深度”相等意味着子对象的可验证的“自己”属性也由以下规则递归计算。
     */
    function deepStrictEqual<T>(actual: unknown, expected: T, message?: string | Error): asserts actual is T;
    /**
     * 【深度不相等】
     * 如果两个值完全相等，则抛出一个`AssertionError`，并将`message`属性设置为等于`message`参数的值。
     * 如果“message”参数未定义，则分配默认错误消息。
     * 如果`message`参数是`Error`的实例，那么它将被抛出，而不是`AssertionError`。
     */
    function notDeepEqual(actual: unknown, expected: unknown, message?: string | Error): void;
    /**
     * 【深度严格不相等】
     * 深度严格不等式的检验与{@link deepaltEqual}相反。
     * 如果两个值完全相等且严格相等，则抛出一个`AssertionError`，并将`message`属性设置为等于`message`参数的值。
     * 如果“message”参数未定义，则分配默认错误消息。
     * 如果`message`参数是`Error`的实例，那么它将被抛出，而不是`AssertionError`。
     */
    function notDeepStrictEqual(actual: unknown, expected: unknown, message?: string | Error): void;
    /**
     * 不匹配抛异常
     *
     * 如果指定，则“error”可以是 
     * [`Class`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes),
     * [`RegExp`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions),
     * 验证函数,
     * 一个验证对象，其中每个属性都将被测试为严格的深度相等,
     * 或者一个错误实例，其中每个属性都将被测试是否严格深度相等，包括不可验证的“message”和“name”属性。
     * 当使用对象时，也可以在验证字符串属性时使用正则表达式。参见下文的示例。
     *
     * 如果指定了，如果`fn`调用失败或错误验证失败，`message`将被附加到`AssertionError`提供的消息中。
     *
     * 由于容易出错的表示法，避免将字符串作为第二个参数。
     */
    function throws(block: () => unknown, message?: string | Error): void;
    function throws(block: () => unknown, error: AssertPredicate, message?: string | Error): void;
    /**
     * 匹配抛异常
     * 使用`assert.doesNotThrow（）`实际上没有用，因为捕获错误然后重新抛出它没有任何好处。 
     * 相反，请考虑在特定的代码路径旁边添加一个注释，该注释不应抛出并使错误消息尽可能具有表达性。
     * 当`assert.doesNotThrow（）`被调用时，它将立即调用`fn`函数。
     * 如果抛出一个错误，并且该错误的类型与`error`参数指定的类型相同，则抛出`AssertionError`。
     * 如果错误属于不同类型， 
     * 或者，如果“error”参数未定义，则错误被传播回调用者。
     * 
     * 如果指定，则“error”可以是
     * [`Class`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes),
     * [`RegExp`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions), 
     * 或验证方法。查看{@link throws}了解更多详细信息。
     */
    function doesNotThrow(block: () => unknown, message?: string | Error): void;
    function doesNotThrow(block: () => unknown, error: AssertPredicate, message?: string | Error): void;
    /**
     * 如果`value`不是`undefined`或`null`，则抛出`value`。
     * 这在测试回调函数中的“error”参数时很有用。 
     * 堆栈跟踪包含传递给`ifError（）`的错误的所有帧，包括`ifError（）`本身的潜在新帧。
     */
    function ifError(value: unknown): asserts value is null | undefined;
    /**
     * 等待`promise`，或者，如果`promise`是一个函数，则立即调用该函数并等待返回的promise完成。 
     * 然后它将检查promise是否被拒绝。
     *
     * 如果`mccFn`是一个函数，它同步抛出一个错误，`assert.rejects（）`将返回一个被拒绝的`Promise`，并带有该错误。 
     * 如果函数没有返回一个promise，`assert.rejects（）`将返回一个被拒绝的`Promise`，并带有一个`ERR_INVALID_RETURN_VALUE`错误。 
     * 在这两种情况下，都会跳过错误处理程序。
     *
     * 除了等待完成的特性外，等待完成的行为与{@link throws}相同。
     *
     * 如果指定，则“error”可以是
     * [`Class`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes),
     * [`RegExp`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions), 
     * 验证函数,
     * 将测试每个属性的对象，或将测试每个属性是否包含不可验证的“message”和“name”属性的错误实例.
     *
     * 如果指定，则`message`将是`AssertionError`在`AssertionFn`拒绝失败时提供的消息。
     * “error”不能是字符串。如果提供了一个字符串作为第二个参数，则假定省略了“error”，该字符串将用于“message”。
     * 这可能导致容易错过的错误。 如果考虑使用字符串作为第二个参数，请仔细阅读{@link throws}中的示例。
     */
    function rejects(block: (() => Promise<unknown>) | Promise<unknown>, message?: string | Error): Promise<void>;
    function rejects(block: (() => Promise<unknown>) | Promise<unknown>, error: AssertPredicate, message?: string | Error): Promise<void>;
    /**
     * 等待`promise`，或者，如果`promise`是一个函数，则立即调用该函数并等待返回的promise完成。然后，它将检查promise是否未被拒绝。
     * 
     * 如果`Promise Fn`是一个函数，并且它同步抛出一个错误，`assert. doesNothing（）`将返回一个被拒绝的带有该错误的`Promise`。 
     * 如果函数没有返回一个promise，`assert. doesNothing（）`将返回一个被拒绝的`Promise`，并带有一个`ERR_INVALID_RETURN_VALUE`错误。
     * 在这两种情况下，都会跳过错误处理程序。
     * 使用`assert. doesNothing（）`实际上没有什么用处，因为捕获拒绝然后再次拒绝它没有什么好处。
     * 相反，请考虑在特定代码路径旁边添加注释，该注释不应拒绝并使错误消息尽可能具有表达性。
     * 
     * 如果指定，则“error”可以是
     * [`Class`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes),
     * [`RegExp`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions), 
     * 或验证功能。查看{@link throws}了解更多详细信息。
     * 
     * 除了等待完成的特性之外，它的行为与{@link doesNotThrow}相同。
     */
    function doesNotReject(block: (() => Promise<unknown>) | Promise<unknown>, message?: string | Error): Promise<void>;
    function doesNotReject(block: (() => Promise<unknown>) | Promise<unknown>, error: AssertPredicate, message?: string | Error): Promise<void>;
    /**
     * 期望`string`输入匹配正则表达式。（匹配抛异常）
     * 如果值不匹配，或者如果`string`参数是`string`以外的其他类型，则抛出`AssertionError`，并将`message`属性设置为等于`message`参数的值。 
     * 如果“message”参数未定义，则分配默认错误消息。 
     * 如果`message`参数是`Error`的实例，那么它将被抛出，而不是`AssertionError`。
     */
    function match(value: string, regExp: RegExp, message?: string | Error): void;
    /**
     * 期望`string`输入与正则表达式不匹配。（不匹配抛异常）
     * 如果值匹配，或者如果`string`参数是`string`以外的其他类型，则抛出`AssertionError`，并将`message`属性设置为等于`message`参数的值。
     * 如果“message”参数未定义，则分配默认错误消息。
     * 如果`message`参数是`Error`的实例，那么它将被抛出，而不是`AssertionError`
     */
    function doesNotMatch(value: string, regExp: RegExp, message?: string | Error): void;
}
```

- 示例1：fail抛异常
```js
const assert = require('node:assert/strict');
assert.fail();// AssertionError [ERR_ASSERTION]: Failed
assert.fail('boom');// AssertionError [ERR_ASSERTION]: boom
assert.fail(new TypeError('need array'));// TypeError: need array
```
- 示例2：ok值为真
```js
const assert = require('node:assert/strict');
assert.ok(true);// OK
assert.ok(1);// OK
assert.ok();// AssertionError: No value argument passed to `assert.ok()`
assert.ok(false, 'it\'s false');// AssertionError: it's false
// 在`repl`中:
assert.ok(typeof 123 === 'string');// AssertionError: false == true
// 在file中 (e.g. test.js):
assert.ok(typeof 123 === 'string');
// AssertionError: The expression evaluated to a falsy value:表达式的计算结果为假值
//
//   assert.ok(typeof 123 === 'string')
assert.ok(false);
// AssertionError: The expression evaluated to a falsy value:表达式的计算结果为假值
//
//   assert.ok(false)
assert.ok(0);
// AssertionError: The expression evaluated to a falsy value:表达式的计算结果为假值
//
//   assert.ok(0)
```
- 示例3：assert值为真
```js
const assert = require('node:assert/strict');
// 使用`assert()`的效果是一样的：
assert(0);
// AssertionError: The expression evaluated to a falsy value:表达式的计算结果为假值
//
//   assert(0)
```
- 示例4：equal相等
```js
import assert from 'node:assert';
assert.equal(1, 1);// OK, 1 == 1
assert.equal(1, '1');// OK, 1 == '1'
assert.equal(NaN, NaN);// OK
assert.equal(1, 2);// AssertionError: 1 == 2
assert.equal({ a: { b: 1 } }, { a: { b: 1 } });
// AssertionError: { a: { b: 1 } } == { a: { b: 1 } }
```
- 示例5：strictEqual相等
```js
const assert = require('node:assert/strict');
assert.strictEqual(1, 2);
// AssertionError [ERR_ASSERTION]: Expected inputs to be strictly equal:
//
// 1 !== 2
assert.strictEqual(1, 1);// OK
assert.strictEqual('Hello foobar', 'Hello World!');
// AssertionError [ERR_ASSERTION]: Expected inputs to be strictly equal:
// + actual - expected
//
// + 'Hello foobar'
// - 'Hello World!'
//          ^
const apples = 1;
const oranges = 2;
assert.strictEqual(apples, oranges, `apples ${apples} !== oranges ${oranges}`);
// AssertionError [ERR_ASSERTION]: apples 1 !== oranges 2
assert.strictEqual(1, '1', new TypeError('Inputs are not identical'));
// TypeError: Inputs are not identical
```
- 示例6：notEqual不相等
```js
import assert from 'node:assert';
assert.notEqual(1, 2);// OK
assert.notEqual(1, 1);// AssertionError: 1 != 1
assert.notEqual(1, '1');// AssertionError: 1 != '1'
```
- 示例7：notStrictEqual不相等
```js
const assert = require('node:assert/strict');
assert.notStrictEqual(1, 2);// OK
assert.notStrictEqual(1, 1);
// AssertionError [ERR_ASSERTION]: Expected "actual" to be strictly unequal to:
//
// 1
assert.notStrictEqual(1, '1');
// OK
```
- 示例8：notDeepEqual深度不相等
```js
import assert from 'node:assert';
const obj1 = {
  a: {
    b: 1,
  },
};
const obj2 = {
  a: {
    b: 2,
  },
};
const obj3 = {
  a: {
    b: 1,
  },
};
const obj4 = { __proto__: obj1 };
assert.notDeepEqual(obj1, obj1);// AssertionError: { a: { b: 1 } } notDeepEqual { a: { b: 1 } }
assert.notDeepEqual(obj1, obj2);// OK
assert.notDeepEqual(obj1, obj3);// AssertionError: { a: { b: 1 } } notDeepEqual { a: { b: 1 } }
assert.notDeepEqual(obj1, obj4);// OK
```
- 示例9：notDeepStrictEqual深度不相等
```js
const assert = require('node:assert/strict');
assert.notDeepStrictEqual({ a: 1 }, { a: '1' });
// OK
```
- 示例10：throws校验属性是否匹配，不匹配抛异常
```js
const assert = require('node:assert/strict');
// 定义error
const err = new TypeError('Wrong value');
err.code = 404;
err.foo = 'bar';
err.info = {
  nested: true,
  baz: 'text',
};
err.reg = /abc/i;
// 将只测试验证对象上的属性，使用嵌套对象要求所有属性都存在。否则，验证将失败。
assert.throws(() => {
    throw err;
  },
  {
    name: 'TypeError',
    message: 'Wrong value',
    info: {
      nested: true,
      baz: 'text',
    },
  }
);
// 使用正则表达式验证错误属性：
assert.throws(
  () => {
    throw err;
  },
  {
    // “name”和“message”属性是字符串，在这些属性上使用正则表达式将与字符串匹配。如果失败，则抛出错误。
    name: /^TypeError$/,
    message: /Wrong/,
    foo: 'bar',
    info: {
      nested: true,
      // 不可能对嵌套属性使用正则表达式！
      baz: 'text',
    },
    // reg属性包含一个正则表达式，只有当验证对象包含一个相同的正则表达式时，它才会通过。
    reg: /abc/i,
  },
);
// 由于`message`和`name`属性不同而失败：
assert.throws(
  () => {
    const otherErr = new Error('Not found');
    // 将所有可复制属性从`err`复制到`otherErr`。
    for (const [key, value] of Object.entries(err)) {
      otherErr[key] = value;
    }
    throw otherErr;
  },
  // 当使用错误作为验证对象时，也将检查错误的`message`和`name`属性。
  err,
);
```
- 示例11：throws使用构造函数校验
```js
const assert = require('node:assert/strict');
assert.throws(
  () => {
    throw new Error('Wrong value');
  },
  Error,
);
```
- 示例12：throws校验对象通过 [`RegExp`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions):
    - 使用正则表达式在错误对象上运行`.toString`，因此也将包含错误名称。
```js
const assert = require('node:assert/strict');
assert.throws(
  () => {
    throw new Error('Wrong value');
  },
  /^Error: Wrong value$/,
);
```
- 示例13：throws自定义错误验证：
    - 该函数必须返回“true”以指示所有内部验证都已通过。
    - 否则，它将失败并返回一个“AssertionError”。
```js
const assert = require('node:assert/strict');
assert.throws(
  () => {
    throw new Error('Wrong value');
  },
  (err) => {
    assert(err instanceof Error);
    assert(/value/.test(err));
// 避免从验证函数中返回除了“true”以外的任何内容。否则，不清楚验证的哪一部分失败了。
// 相反，抛出一个关于失败的特定验证的错误（如本例所示），并向该错误添加尽可能多的有用调试信息。
    return true;
  },
  'unexpected error',
);
```
- 示例14：“error”不能是字符串。如果提供字符串作为第二个参数， 然后假定省略“error”，字符串将用于“message”。 
    - 这可能导致容易错过的错误。
    - 使用与抛出的错误消息相同的消息将导致`ERR_AMBIGUOUS_ARGUMENT`错误。 
    - 如果考虑使用字符串作为第二个参数，请仔细阅读下面的示例：
```js
const assert = require('node:assert/strict');
function throwingFirst() {
  throw new Error('First');
}
function throwingSecond() {
  throw new Error('Second');
}
function notThrowing() {}
// 第二个参数是一个字符串，输入函数抛出了一个错误。 
// 第一种情况不会抛出，因为它与输入函数抛出的错误消息不匹配！
assert.throws(throwingFirst, 'Second');

// 在下一个示例中，消息没有比错误消息更好的效果，
// 因为不清楚用户是否打算实际匹配错误消息，Node.js抛出了一个`ERR_AMBIGUOUS_ARGUMENT`错误。
assert.throws(throwingSecond, 'Second');
// TypeError [ERR_AMBIGUOUS_ARGUMENT]

// 字符串仅在函数不抛出的情况下使用（作为消息）：
assert.throws(notThrowing, 'Second');
// AssertionError [ERR_ASSERTION]：Missing expected exception: Second

// 如果要匹配错误消息，请执行以下操作： 它不会抛出，因为错误消息匹配。
assert.throws(throwingSecond, /Second$/);

// 如果错误消息不匹配，则抛出AssertionError。
assert.throws(throwingFirst, /Second$/);
// AssertionError [ERR_ASSERTION]
```
- 下面的代码将抛出`TypeError`，因为断言中没有匹配的错误类型：
```js
const assert = require('node:assert/strict');
assert.doesNotThrow(
  () => {
    throw new TypeError('Wrong value');
  },
  SyntaxError,
);
```
- 但是，以下操作将导致一个“AssertionError”，并显示消息“Got unwanted exception. '：
```js
const assert = require('node:assert/strict');
assert.doesNotThrow(
  () => {
    throw new TypeError('Wrong value');
  },
  TypeError,
);
```
- 如果抛出`AssertionError`，并为`message`参数提供了值，则`message`的值将被追加到`AssertionError`消息中：
```js
const assert = require('node:assert/strict');
assert.doesNotThrow(
  () => {
    throw new TypeError('Wrong value');
  },
  /Wrong value/,
  'Whoops',
);
// Throws: AssertionError: Got unwanted exception: Whoops
```
- ifError不为空抛异常
```js
const assert = require('node:assert/strict');
assert.ifError(null);// OK

assert.ifError(0);
// AssertionError [ERR_ASSERTION]: ifError got unwanted exception: 0
assert.ifError('error');
// AssertionError [ERR_ASSERTION]: ifError got unwanted exception: 'error'
assert.ifError(new Error());
// AssertionError [ERR_ASSERTION]: ifError got unwanted exception: Error

// Create some random error frames.
let err;
(function errorFrame() {
  err = new Error('test error');
})();
(function ifErrorFrame() {
  assert.ifError(err);
})();
// AssertionError [ERR_ASSERTION]: ifError got unwanted exception: test error
//     at ifErrorFrame
//     at errorFrame
```
- 判断Promise被拒绝的结果，是否和目标一致
```js
const assert = require('node:assert/strict');
await assert.rejects(
  async () => {
    throw new TypeError('Wrong value');
  },
  {
    name: 'TypeError',
    message: 'Wrong value',
  },
);
```
```js
const assert = require('node:assert/strict');
await assert.rejects(
  async () => {
    throw new TypeError('Wrong value');
  },
  (err) => {
    assert.strictEqual(err.name, 'TypeError');
    assert.strictEqual(err.message, 'Wrong value');
    return true;
  },
);
```
```js
const assert = require('node:assert/strict');
assert.rejects(
  Promise.reject(new Error('Wrong value')),
  Error,
).then(() => {
  // ...
});
```
- 判断Promise被拒绝的结果，是否和目标不一致
```js
const assert = require('node:assert/strict');
await assert.doesNotReject(
  async () => {
    throw new TypeError('Wrong value');
  },
  SyntaxError,
);
```
```js
const assert = require('node:assert/strict');
assert.doesNotReject(Promise.reject(new TypeError('Wrong value')))
  .then(() => {
    // ...
  });
```
- 是否匹配正则表达式，不匹配抛异常
```js
const assert = require('node:assert/strict');
assert.match('I will fail', /pass/);
// AssertionError [ERR_ASSERTION]: The input did not match the regular ...
assert.match(123, /pass/);
// AssertionError [ERR_ASSERTION]: The "string" argument must be of type string.
assert.match('I will pass', /pass/);
// OK
```
- 是否匹配正则表达式，匹配抛异常
```js
const assert = require('node:assert/strict');
assert.doesNotMatch('I will fail', /fail/);
// AssertionError [ERR_ASSERTION]: The input was expected to not match the ...
assert.doesNotMatch(123, /pass/);
// AssertionError [ERR_ASSERTION]: The "string" argument must be of type string.
assert.doesNotMatch('I will pass', /different/);
// OK
```
