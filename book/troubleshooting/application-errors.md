
## Application Errors

### Istio/Envoy

#### Upstream Connect error

> upstream connect error or disconnect/reset before headers. reset
> reason: connection termination

This error is returned by Envoy if your web server disconnects before
sending a response. This could happen if:

  - Your web process crashes or exits in the middle of a request.

  - Your web container is killed due to excessive memory usage.

  - Your web container is removed during a deploy or scale down event
    and doesn't gracefully handle termination.

If this error is returned consistently by performing the same request,
it's likely that something in the response is causing your application
server to crash. Inspect the application logs to determine where the
crash may be happening.

This error is most likely caused by an application crash and not by a
problem in the infrastructure.

### Puma

#### Unable to add work while shutting down

This error occurs when Puma receives a request after the Puma server has
received a SIGTERM signal.

If you receive this error, first ensure you upgrade Puma to version 5 or
greater, as there is a [fix](https://github.com/puma/puma/pull/2122) for
a bug in Puma itself.

This can still happen in large Kubernetes deployments because of the way
[pod
termination](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#pod-termination)
works. When a pod is scheduled to be terminated, Kubernetes will
simultaneously send a SIGTERM to pod's containers and schedule the pod's
[endpoints](https://stackoverflow.com/questions/52857825/what-is-an-endpoint-in-kubernetes)
to be removed. If a request arrives after SIGTERM is received but before
the endpoints are removed, the request can be routed to a pod in a
terminating state.

If you're still receiving this error after upgrading Puma, you can add a
delay to your pod termination workflow to provide enough time for
endpoints to be removed.

Add the following to your web container manifest:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Add to spec.template.spec.containers in your web deployment
lifecycle:
  preStop:
    exec:
      command: ["/bin/sleep","30"]
```

</div>

</div>

Then increase your pod's termination grace period to avoid receiving
SIGKILL:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
# Add to spec.template.spec in your web deployment
terminationGracePeriodSeconds: 60
```

</div>

</div>

For more information, see [Puma documentation for
Kubernetes](https://github.com/puma/puma/blob/master/docs/kubernetes.md#graceful-shutdown-and-pod-termination).

### RDS/Postgres

#### Too Many Connections

You may see one of these errors:

> PG::ConnectionBad: FATAL: remaining connection slots are reserved for
> non-replication superuser connections

> ActiveRecord::NoDatabaseError: FATAL: sorry, too many clients already

This means that you have already used the maximum number of connections
for your Postgres instance. The maximumum number of connections varies
based on the size of the instance.

You can use a console to check the number of database connections:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
% flightctl console rails dbconsole -p
postgres=> show max_connections;
 max_connections
-----------------
 83
```

</div>

</div>

Note that some connections will be reserved for superusers:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
postgres=> show superuser_reserved_connections;
 superuser_reserved_connections
--------------------------------
 3
```

</div>

</div>

In this example, the application is allowed to use at most 80
connections.

In order to resolve this issue, you can:

  - Reduce the number of replicas running for one of your services to
    free up its connections.

  - Reduce the number of connections used by each replica.

  - Upgrade your database connection to allow for more connections.

  - Add a connection proxy to load balance connections to your database.

  - Change the `max_connections` setting in Postgres if you're sure it
    won't cause out of memory errors for your use case.
