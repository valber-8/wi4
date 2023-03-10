# WI4 repository based on microfab project

'microfab' provides a single container image that allows you to quickly build, operate & govern and grow blockchain networks. It uses Hyperledger Fabric, the open source, industry standard for enterprise blockchain. 

This containerized version of Fabric can be easily configured with the selection of channels and orgs you want, and also can be started and stopped in seconds.  You can interfact with it as you would any Fabric setup. Note that this uses *the* fabric binaries and starts Fabric with couchdb and cas for identities. It's not cut down.

## Starting 

To start ledger with the default configuration using Docker, run the following command:

    docker run -p 8080:8080 wi4

Microfab provides a REST API. This REST API provides all the information you need to connect to the Hyperledger Fabric runtime using any of the Hyperledger Fabric SDKs.

To access this information, use the following REST API:

    curl http://console.127.0.0.1.nip.io:8080/ak/api/v1/components

Connection profiles are returned with a type of `gateway`:

    curl http://console.127.0.0.1.nip.io:8080/ak/api/v1/components | jq '.[] | select(.type == "gateway")'

Identities (certificate and private key pairs) are returned with a type of `identity`:

    curl http://console.127.0.0.1.nip.io:8080/ak/api/v1/components | jq '.[] | select(.type == "identity")'

## Configuration

The configuration is a JSON object with the following keys:

- `domain`

  The domain name to use. The domain name must be resolvable both outside and inside the container, and it must resolve to an IP address of that container (or the system hosting the container).

  Default value: `"127-0-0-1.nip.io"`

- `port`

  The port to use. The port must be accessible both outside and inside the container.

  Default value: `8080`

- `directory`

  The directory to store data in within the container.

  Default value: `"/home/microfab/data"`

- `ordering_organization`

  The ordering organization.

  Default value:

      {
        "name": "Orderer" // The name of the organization.
      }

- `endorsing_organizations`

  The list of endorsing organizations.

  Default value:

      [
        {
          "name": "Org1" // The name of the organization.
        }
      ]

- `channels`

  The list of channels.

  Default value:

      [
        {
          "name": "channel1", // The name of the channel.
          "endorsing_organizations": [ // The list of endorsing organizations that are members of the channel.
            "Org1"
          ],
          "capability_level": "V2_0" // Optional: the application capability level of the channel.
        }
      ]

- `capability_level`

  The application capability level of all channels. Can be overriden on a per-channel basis.

  Default value: `"V2_0"`

- `couchdb`

  Whether or not to use CouchDB as the world state database.

  Default value: `true`

- `certificate_authorities`

  Whether or not to create certificate authorities for all endorsing organizations.

  Default value: `true`

- `timeout`

  The time to wait for all components to start.

  Default value: `"30s"`

- `tls`

  The TLS configuration.

  Default value:

      {
        "enabled": false, // Set to true to enable TLS.
        "certificate": null, // Optional: the TLS certificate to be used.
        "private_key": null, // Optional: the TLS private key to be used.
        "ca": null // Optional: the TLS CA certificate to be used.
      }



## License

Apache-2.0

## Author Information

