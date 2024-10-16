# Connecting To Networks

We call the initial node (e.g. $N_A$) of a Tangle network the *bootstrap node*.

In order for another node to join (e.g. $N_B$), the node must know the IP address of the bootstrap node.

## Join Request

If $N_B$ would like to join $N_A$'s network, it must first send a join request:

```
{
  type: "t.net.joinRequest"
}
```

If $N_A$ has any network policies set up, $N_B$ will need to solve a [join challenge](#join-challenge).

If no network policies are present, $N_A$ will answer with a [join result](#join-result).

## Join Challenge

The join challenge is dependent on the [network policies]() configured on the *bootstrap server*.

For a passphrase-based join challenge, $N_A$ will answer with the following:

```
{
  type: "t.net.joinChallenge"
  payload: {
    available: ["passphrase"],
    required: 1
  }
}
```

$N_B$ can reply with any of the keys found within `payload.available` in it's own payload.

```
{
  type: "t.net.joinRequest",
  payload: {
    passphrase: "HelloWorld123!"
  }
}
```

The number of challenges done must match `payload.required`, otherwise, the request will be declined.

## Join Result
