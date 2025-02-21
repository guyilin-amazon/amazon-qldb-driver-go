# 2.0.2 (2021-07-21)

Releases v2.0.0 and v2.0.1 were skipped due to instability issues.

## :tada: Enhancements

* Bumped Ion Go dependency to `v1.1.3` allow support for unmarshalling Ion timestamps to Go time objects.

## :boom: Breaking changes

* The `Logger` interface's `Log` method now takes in a `LogLevel` to specify the logging verbosity. Any instances of `Logger.Log()` will need to be updated accordingly.

    ie.
    ```go
    logger.Log("Log Message")
    ```

    should be updated to

    ```go
    logger.Log("Log Message", qldbdriver.LogInfo)
    ```

* `Result` and `BufferedResult` have changed from struct types to interface types. As a consequence of this change, the `Transaction` interface's `Execute()` and `BufferResult()` methods respectively return `Result` and `BufferedResult` rather than `*Result` and `*BufferedResult`. Any logic dereferencing or casting to a `Result` or `BufferedResult` will need to be updated accordingly.

    ie.
    ```go
    result.(*BufferedResult)
    ```

    should be updated to

    ```go
    result.(BufferedResult)
    ```

* The `Transaction` interface has a new `ID()` method for exposing the transaction ID. Any implementations of this interface will need a new `ID() string` method defined.

# 1.1.1 (2021-06-16)

## :bug: Fixed

* Bumped Ion Go to `v1.1.2` and Ion Hash Go to `v1.1.1` to fix a bug where inserting certain timestamps would throw an error.
* Prevent mutation of `*qldbsession.QLDBSession` after passing into QLDBDriver constructor.
* Allow users to wrap `awserr.RequestFailure` within a transaction lambda and support retryable errors.

# 1.1.0 (2021-02-25)

## :tada: Enhancements

* Updated the AWS SDK dependency to v1.37.8 to support [CapacityExceededException](https://docs.aws.amazon.com/qldb/latest/developerguide/driver-errors.html). This exception will better inform users that they are overloading their ledger.
* Statements that return exceptions containing status code 500 and 503 will now be retried.
* User agent string is now included in start session requests.
* Added `IOUsage` and `TimingInformation` structs to provide server-side execution statistics
    * `IOUsage` provides `GetReadIOs` method
    * `TimingInformation` provides `GetProcessingTimeMilliseconds` method
    * Added `GetConsumedIOs` and `GetTimingInformation` methods in `BufferedResult` and `Result` structs
    * `GetConsumedIOs` and `GetTimingInformation` methods are stateful, meaning the statistics returned by them reflect the state at the time of method execution.

# 1.0.1 (2020-10-27)

* Fixed the dates in this CHANGELOG.md file
* Updated the driver version in the user agent string to 1.0.1.

# 1.0.0 (2020-10-20)

The release candidate (v1.0.0-rc.1) has been selected as a final release of v1.0.0 with the following change:

## :boom: Breaking changes

* `QLDBDriverError` struct is no longer exported and has been updated to `qldbDriverError`.

# 1.0.0-rc.1 (2020-10-06)

## :tada: Enhancements

* Improved the iterator pattern for `Result.Next`. More details can be found in the [release notes](https://github.com/awslabs/amazon-qldb-driver-go/releases/tag/v1.0.0-rc.1)
* Removed panics in the driver. We can handle more errors gracefully now.
  
## :boom: Breaking changes

* Updated `QldbDriver.New` function to return `(QLDBDriver, error)`.
* Renamed `QldbDriver.Close` function to `QldbDriver.Shutdown`.
* Removed `QLDBDriver.ExecuteWithRetryPolicy` function.
* Removed `RetryPolicyContext` struct. `BackoffStrategy.Delay` function now takes in an `int` parameter as retry attempt.
* The `SleepBase` and `SleepCap` fields in struct type `ExponentialBackoffStrategy` have been updated to type `time.Duration`.

# 0.1.0 (2020-08-06)

* Preview release of the driver.
