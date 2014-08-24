# TACK - Telehash Alias Canonical toKen

TACK is a globally unique resolvable identifier for use within any system that supports telehash.  It defines a URI format that any endpoint can use to exchange temporary or permanent connectivity information with any other.  Once a TACK is resolved, the endpoint information (keys and paths) should be used in its place, they are only used successfully once.

The TACK takes the form: `tack:alias@canonical/token`

* `tack:` - fixed uri protocol
* `alias` - optional, friendly name
* `canonical` - required, valid host name format
* `token` - optional

The alias and token must be base32 encoded if they contain any binary or special characters (TBD) and prefixed with a + to indicate such.

Routers return their canonical name and one or more aliases if supported.  Any endpoint's hashname can be used as the alias on a router.

If routers may use [dotPublic](https://github.com/quartzjer/dotPublic) instead of a registered domain name.

Token is used to generate tack's that can be uniquely tracked to authorize new connections or specify permissions.

Processing:

1. validate canonical
2. is canonical us
3. do we know canonical
4. if not, can we ask the tack source to peer just the canonical
5. can we resolve the canonical

## DNS

SRV records always resolve to a hashname-prefixed host, with TXT records returning all of the keys.

* `_tack._udp.example.com. 86400 IN SRV 0 5 42424 uvabrvfqacyvgcu8kbrrmk9apjbvgvn2wjechqr3vf9c1zm3hv7g.example.com.` (also optionally support `_tcp` and `_http`, etc)
* `uvabrvfqacyvgcu8kbrrmk9apjbvgvn2wjechqr3vf9c1zm3hv7g IN A 1.2.3.4`
* `uvabrvfqacyvgcu8kbrrmk9apjbvgvn2wjechqr3vf9c1zm3hv7g IN TXT "1a=base32"`
* `uvabrvfqacyvgcu8kbrrmk9apjbvgvn2wjechqr3vf9c1zm3hv7g IN TXT "2a=base32"`
* `uvabrvfqacyvgcu8kbrrmk9apjbvgvn2wjechqr3vf9c1zm3hv7g IN TXT "2a2=base32"`
* `uvabrvfqacyvgcu8kbrrmk9apjbvgvn2wjechqr3vf9c1zm3hv7g IN TXT "3a=base32"`

If a key's base32 encoding is larger than 250 characters, it is broken into multiple TXT records with the CSID being numerically increased so that it can be easily reassembled.

## HTTP well-known

```
GET http://example.com/.well-known/tack.json
{
  "keys":{...},
  "paths":{...}
}