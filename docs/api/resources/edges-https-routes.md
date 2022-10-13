import EdgesHTTPSRoutesCreateRequest from './examples/_edges_https_routes_create_request.md';
import EdgesHTTPSRoutesCreateResponse from './examples/_edges_https_routes_create_response.md';
import EdgesHTTPSRoutesGetRequest from './examples/_edges_https_routes_get_request.md';
import EdgesHTTPSRoutesGetResponse from './examples/_edges_https_routes_get_response.md';
import EdgesHTTPSRoutesUpdateRequest from './examples/_edges_https_routes_update_request.md';
import EdgesHTTPSRoutesUpdateResponse from './examples/_edges_https_routes_update_response.md';
import EdgesHTTPSRoutesDeleteRequest from './examples/_edges_https_routes_delete_request.md';

#  Edge HTTPS Routes
------------


## Create HTTPS Edge Route
Create an HTTPS Edge Route

### Request


POST /edges/https/{edge_id}/routes

<EdgesHTTPSRoutesCreateRequest />


#### Parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `edge_id` | string | unique identifier of this edge |
| `match_type` | string | Type of match to use for this route. Valid values are "exact_path" and "path_prefix". |
| `match` | string | Route selector: "/blog" or "example.com" or "example.com/blog" |
| `description` | string | human-readable description of what this edge will be used for; optional, max 255 bytes. |
| `metadata` | string | arbitrary user-defined machine-readable data of this edge. Optional, max 4096 bytes. |
| `backend` | [EndpointBackendMutate](#api-edges-https-routes-create-parameters-endpoint-backend-mutate) | backend module configuration or `null` |
| `ip_restriction` | [EndpointIPPolicyMutate](#api-edges-https-routes-create-parameters-endpoint-ip-policy-mutate) | ip restriction module configuration or `null` |
| `circuit_breaker` | [EndpointCircuitBreaker](#api-edges-https-routes-create-parameters-endpoint-circuit-breaker) | circuit breaker module configuration or `null` |
| `compression` | [EndpointCompression](#api-edges-https-routes-create-parameters-endpoint-compression) | compression module configuration or `null` |
| `request_headers` | [EndpointRequestHeaders](#api-edges-https-routes-create-parameters-endpoint-request-headers) | request headers module configuration or `null` |
| `response_headers` | [EndpointResponseHeaders](#api-edges-https-routes-create-parameters-endpoint-response-headers) | response headers module configuration or `null` |
| `webhook_verification` | [EndpointWebhookValidation](#api-edges-https-routes-create-parameters-endpoint-webhook-validation) | webhook verification module configuration or `null` |
| `oauth` | [EndpointOAuth](#api-edges-https-routes-create-parameters-endpoint-o-auth) | oauth module configuration or `null` |
| `saml` | [EndpointSAMLMutate](#api-edges-https-routes-create-parameters-endpoint-saml-mutate) | saml module configuration or `null` |
| `oidc` | [EndpointOIDC](#api-edges-https-routes-create-parameters-endpoint-oidc) | oidc module configuration or `null` |
| `websocket_tcp_converter` | [EndpointWebsocketTCPConverter](#api-edges-https-routes-create-parameters-endpoint-websocket-tcp-converter) | websocket to tcp adapter configuration or `null` |

#### EndpointBackendMutate parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `backend_id` | string | backend to be used to back this endpoint |

#### EndpointIPPolicyMutate parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `ip_policy_ids` | List&lt;string&gt; | list of all IP policies that will be used to check if a source IP is allowed access to the endpoint |

#### EndpointCircuitBreaker parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `tripped_duration` | uint32 | Integer number of seconds after which the circuit is tripped to wait before re-evaluating upstream health |
| `rolling_window` | uint32 | Integer number of seconds in the statistical rolling window that metrics are retained for. |
| `num_buckets` | uint32 | Integer number of buckets into which metrics are retained. Max 128. |
| `volume_threshold` | uint32 | Integer number of requests in a rolling window that will trip the circuit. Helpful if traffic volume is low. |
| `error_threshold_percentage` | float64 | Error threshold percentage should be between 0 - 1.0, not 0-100.0 |

#### EndpointCompression parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |

#### EndpointRequestHeaders parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `add` | Map&lt;string, string&gt; | a map of header key to header value that will be injected into the HTTP Request before being sent to the upstream application server |
| `remove` | List&lt;string&gt; | a list of header names that will be removed from the HTTP Request before being sent to the upstream application server |

#### EndpointResponseHeaders parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `add` | Map&lt;string, string&gt; | a map of header key to header value that will be injected into the HTTP Response returned to the HTTP client |
| `remove` | List&lt;string&gt; | a list of header names that will be removed from the HTTP Response returned to the HTTP client |

#### EndpointWebhookValidation parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `provider` | string | a string indicating which webhook provider will be sending webhooks to this endpoint. Value must be one of the supported providers defined at https://ngrok.com/docs/cloud-edge#webhook-verification |
| `secret` | string | a string secret used to validate requests from the given provider. All providers except AWS SNS require a secret |

#### EndpointOAuth parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `provider` | [EndpointOAuthProvider](#api-edges-https-routes-create-parameters-endpoint-o-auth-provider) | an object which defines the identity provider to use for authentication and configuration for who may access the endpoint |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `auth_check_interval` | uint32 | Integer number of seconds after which ngrok guarantees it will refresh user state from the identity provider and recheck whether the user is still authorized to access the endpoint. This is the preferred tunable to use to enforce a minimum amount of time after which a revoked user will no longer be able to access the resource. |

#### EndpointOAuthProvider parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `github` | [EndpointOAuthGitHub](#api-edges-https-routes-create-parameters-endpoint-o-auth-git-hub) | configuration for using github as the identity provider |
| `facebook` | [EndpointOAuthFacebook](#api-edges-https-routes-create-parameters-endpoint-o-auth-facebook) | configuration for using facebook as the identity provider |
| `microsoft` | [EndpointOAuthMicrosoft](#api-edges-https-routes-create-parameters-endpoint-o-auth-microsoft) | configuration for using microsoft as the identity provider |
| `google` | [EndpointOAuthGoogle](#api-edges-https-routes-create-parameters-endpoint-o-auth-google) | configuration for using google as the identity provider |
| `linkedin` | [EndpointOAuthLinkedIn](#api-edges-https-routes-create-parameters-endpoint-o-auth-linked-in) | configuration for using linkedin as the identity provider |
| `gitlab` | [EndpointOAuthGitLab](#api-edges-https-routes-create-parameters-endpoint-o-auth-git-lab) | configuration for using gitlab as the identity provider |

#### EndpointOAuthGitHub parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |
| `teams` | List&lt;string&gt; | a list of github teams identifiers. users will be allowed access to the endpoint if they are a member of any of these teams. identifiers should be in the 'slug' format qualified with the org name, e.g. `org-name/team-name` |
| `organizations` | List&lt;string&gt; | a list of github org identifiers. users who are members of any of the listed organizations will be allowed access. identifiers should be the organization's 'slug' |

#### EndpointOAuthFacebook parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthMicrosoft parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthGoogle parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthLinkedIn parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string |  |
| `client_secret` | string |  |
| `scopes` | List&lt;string&gt; |  |
| `email_addresses` | List&lt;string&gt; |  |
| `email_domains` | List&lt;string&gt; |  |

#### EndpointOAuthGitLab parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string |  |
| `client_secret` | string |  |
| `scopes` | List&lt;string&gt; |  |
| `email_addresses` | List&lt;string&gt; |  |
| `email_domains` | List&lt;string&gt; |  |

#### EndpointSAMLMutate parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `idp_metadata` | string | The full XML IdP EntityDescriptor. Your IdP may provide this to you as a a file to download or as a URL. |
| `force_authn` | boolean | If true, indicates that whenever we redirect a user to the IdP for authentication that the IdP must prompt the user for authentication credentials even if the user already has a valid session with the IdP. |
| `allow_idp_initiated` | boolean | If true, the IdP may initiate a login directly (e.g. the user does not need to visit the endpoint first and then be redirected). The IdP should set the `RelayState` parameter to the target URL of the resource they want the user to be redirected to after the SAML login assertion has been processed. |
| `authorized_groups` | List&lt;string&gt; | If present, only users who are a member of one of the listed groups may access the target endpoint. |
| `nameid_format` | string | Defines the name identifier format the SP expects the IdP to use in its assertions to identify subjects. If unspecified, a default value of `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent` will be used. A subset of the allowed values enumerated by the SAML specification are supported. |

#### EndpointOIDC parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `issuer` | string | URL of the OIDC "OpenID provider". This is the base URL used for discovery. |
| `client_id` | string | The OIDC app's client ID and OIDC audience. |
| `client_secret` | string | The OIDC app's client secret. |
| `scopes` | List&lt;string&gt; | The set of scopes to request from the OIDC identity provider. |

#### EndpointWebsocketTCPConverter parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |


### Response


Returns a 201 response  on success

<EdgesHTTPSRoutesCreateResponse />


#### Fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `edge_id` | string | unique identifier of this edge |
| `id` | string | unique identifier of this edge route |
| `created_at` | string | timestamp when the edge configuration was created, RFC 3339 format |
| `match_type` | string | Type of match to use for this route. Valid values are "exact_path" and "path_prefix". |
| `match` | string | Route selector: "/blog" or "example.com" or "example.com/blog" |
| `uri` | string | URI of the edge API resource |
| `description` | string | human-readable description of what this edge will be used for; optional, max 255 bytes. |
| `metadata` | string | arbitrary user-defined machine-readable data of this edge. Optional, max 4096 bytes. |
| `backend` | [EndpointBackend](#api-edges-https-routes-create-fields-endpoint-backend) | backend module configuration or `null` |
| `ip_restriction` | [EndpointIPPolicy](#api-edges-https-routes-create-fields-endpoint-ip-policy) | ip restriction module configuration or `null` |
| `circuit_breaker` | [EndpointCircuitBreaker](#api-edges-https-routes-create-fields-endpoint-circuit-breaker) | circuit breaker module configuration or `null` |
| `compression` | [EndpointCompression](#api-edges-https-routes-create-fields-endpoint-compression) | compression module configuration or `null` |
| `request_headers` | [EndpointRequestHeaders](#api-edges-https-routes-create-fields-endpoint-request-headers) | request headers module configuration or `null` |
| `response_headers` | [EndpointResponseHeaders](#api-edges-https-routes-create-fields-endpoint-response-headers) | response headers module configuration or `null` |
| `webhook_verification` | [EndpointWebhookValidation](#api-edges-https-routes-create-fields-endpoint-webhook-validation) | webhook verification module configuration or `null` |
| `oauth` | [EndpointOAuth](#api-edges-https-routes-create-fields-endpoint-o-auth) | oauth module configuration or `null` |
| `saml` | [EndpointSAML](#api-edges-https-routes-create-fields-endpoint-saml) | saml module configuration or `null` |
| `oidc` | [EndpointOIDC](#api-edges-https-routes-create-fields-endpoint-oidc) | oidc module configuration or `null` |
| `websocket_tcp_converter` | [EndpointWebsocketTCPConverter](#api-edges-https-routes-create-fields-endpoint-websocket-tcp-converter) | websocket to tcp adapter configuration or `null` |

#### EndpointBackend fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `backend` | [Ref](#api-edges-https-routes-create-fields-ref) | backend to be used to back this endpoint |

#### Ref fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `id` | string | a resource identifier |
| `uri` | string | a uri for locating a resource |

#### EndpointIPPolicy fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `ip_policies` | [Ref](#api-edges-https-routes-create-fields-ref) |  |

#### EndpointCircuitBreaker fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `tripped_duration` | uint32 | Integer number of seconds after which the circuit is tripped to wait before re-evaluating upstream health |
| `rolling_window` | uint32 | Integer number of seconds in the statistical rolling window that metrics are retained for. |
| `num_buckets` | uint32 | Integer number of buckets into which metrics are retained. Max 128. |
| `volume_threshold` | uint32 | Integer number of requests in a rolling window that will trip the circuit. Helpful if traffic volume is low. |
| `error_threshold_percentage` | float64 | Error threshold percentage should be between 0 - 1.0, not 0-100.0 |

#### EndpointCompression fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |

#### EndpointRequestHeaders fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `add` | Map&lt;string, string&gt; | a map of header key to header value that will be injected into the HTTP Request before being sent to the upstream application server |
| `remove` | List&lt;string&gt; | a list of header names that will be removed from the HTTP Request before being sent to the upstream application server |

#### EndpointResponseHeaders fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `add` | Map&lt;string, string&gt; | a map of header key to header value that will be injected into the HTTP Response returned to the HTTP client |
| `remove` | List&lt;string&gt; | a list of header names that will be removed from the HTTP Response returned to the HTTP client |

#### EndpointWebhookValidation fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `provider` | string | a string indicating which webhook provider will be sending webhooks to this endpoint. Value must be one of the supported providers defined at https://ngrok.com/docs/cloud-edge#webhook-verification |
| `secret` | string | a string secret used to validate requests from the given provider. All providers except AWS SNS require a secret |

#### EndpointOAuth fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `provider` | [EndpointOAuthProvider](#api-edges-https-routes-create-fields-endpoint-o-auth-provider) | an object which defines the identity provider to use for authentication and configuration for who may access the endpoint |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `auth_check_interval` | uint32 | Integer number of seconds after which ngrok guarantees it will refresh user state from the identity provider and recheck whether the user is still authorized to access the endpoint. This is the preferred tunable to use to enforce a minimum amount of time after which a revoked user will no longer be able to access the resource. |

#### EndpointOAuthProvider fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `github` | [EndpointOAuthGitHub](#api-edges-https-routes-create-fields-endpoint-o-auth-git-hub) | configuration for using github as the identity provider |
| `facebook` | [EndpointOAuthFacebook](#api-edges-https-routes-create-fields-endpoint-o-auth-facebook) | configuration for using facebook as the identity provider |
| `microsoft` | [EndpointOAuthMicrosoft](#api-edges-https-routes-create-fields-endpoint-o-auth-microsoft) | configuration for using microsoft as the identity provider |
| `google` | [EndpointOAuthGoogle](#api-edges-https-routes-create-fields-endpoint-o-auth-google) | configuration for using google as the identity provider |
| `linkedin` | [EndpointOAuthLinkedIn](#api-edges-https-routes-create-fields-endpoint-o-auth-linked-in) | configuration for using linkedin as the identity provider |
| `gitlab` | [EndpointOAuthGitLab](#api-edges-https-routes-create-fields-endpoint-o-auth-git-lab) | configuration for using gitlab as the identity provider |

#### EndpointOAuthGitHub fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |
| `teams` | List&lt;string&gt; | a list of github teams identifiers. users will be allowed access to the endpoint if they are a member of any of these teams. identifiers should be in the 'slug' format qualified with the org name, e.g. `org-name/team-name` |
| `organizations` | List&lt;string&gt; | a list of github org identifiers. users who are members of any of the listed organizations will be allowed access. identifiers should be the organization's 'slug' |

#### EndpointOAuthFacebook fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthMicrosoft fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthGoogle fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthLinkedIn fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string |  |
| `client_secret` | string |  |
| `scopes` | List&lt;string&gt; |  |
| `email_addresses` | List&lt;string&gt; |  |
| `email_domains` | List&lt;string&gt; |  |

#### EndpointOAuthGitLab fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string |  |
| `client_secret` | string |  |
| `scopes` | List&lt;string&gt; |  |
| `email_addresses` | List&lt;string&gt; |  |
| `email_domains` | List&lt;string&gt; |  |

#### EndpointSAML fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `idp_metadata` | string | The full XML IdP EntityDescriptor. Your IdP may provide this to you as a a file to download or as a URL. |
| `force_authn` | boolean | If true, indicates that whenever we redirect a user to the IdP for authentication that the IdP must prompt the user for authentication credentials even if the user already has a valid session with the IdP. |
| `allow_idp_initiated` | boolean | If true, the IdP may initiate a login directly (e.g. the user does not need to visit the endpoint first and then be redirected). The IdP should set the `RelayState` parameter to the target URL of the resource they want the user to be redirected to after the SAML login assertion has been processed. |
| `authorized_groups` | List&lt;string&gt; | If present, only users who are a member of one of the listed groups may access the target endpoint. |
| `entity_id` | string | The SP Entity's unique ID. This always takes the form of a URL. In ngrok's implementation, this URL is the same as the metadata URL. This will need to be specified to the IdP as configuration. |
| `assertion_consumer_service_url` | string | The public URL of the SP's Assertion Consumer Service. This is where the IdP will redirect to during an authentication flow. This will need to be specified to the IdP as configuration. |
| `single_logout_url` | string | The public URL of the SP's Single Logout Service. This is where the IdP will redirect to during a single logout flow. This will optionally need to be specified to the IdP as configuration. |
| `request_signing_certificate_pem` | string | PEM-encoded x.509 certificate of the key pair that is used to sign all SAML requests that the ngrok SP makes to the IdP. Many IdPs do not support request signing verification, but we highly recommend specifying this in the IdP's configuration if it is supported. |
| `metadata_url` | string | A public URL where the SP's metadata is hosted. If an IdP supports dynamic configuration, this is the URL it can use to retrieve the SP metadata. |
| `nameid_format` | string | Defines the name identifier format the SP expects the IdP to use in its assertions to identify subjects. If unspecified, a default value of `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent` will be used. A subset of the allowed values enumerated by the SAML specification are supported. |

#### EndpointOIDC fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `issuer` | string | URL of the OIDC "OpenID provider". This is the base URL used for discovery. |
| `client_id` | string | The OIDC app's client ID and OIDC audience. |
| `client_secret` | string | The OIDC app's client secret. |
| `scopes` | List&lt;string&gt; | The set of scopes to request from the OIDC identity provider. |

#### EndpointWebsocketTCPConverter fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |


## Get HTTPS Edge Route
Get an HTTPS Edge Route by ID

### Request


GET /edges/https/{edge_id}/routes/{id}

<EdgesHTTPSRoutesGetRequest />


### Response


Returns a 200 response  on success

<EdgesHTTPSRoutesGetResponse />


#### Fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `edge_id` | string | unique identifier of this edge |
| `id` | string | unique identifier of this edge route |
| `created_at` | string | timestamp when the edge configuration was created, RFC 3339 format |
| `match_type` | string | Type of match to use for this route. Valid values are "exact_path" and "path_prefix". |
| `match` | string | Route selector: "/blog" or "example.com" or "example.com/blog" |
| `uri` | string | URI of the edge API resource |
| `description` | string | human-readable description of what this edge will be used for; optional, max 255 bytes. |
| `metadata` | string | arbitrary user-defined machine-readable data of this edge. Optional, max 4096 bytes. |
| `backend` | [EndpointBackend](#api-edges-https-routes-get-fields-endpoint-backend) | backend module configuration or `null` |
| `ip_restriction` | [EndpointIPPolicy](#api-edges-https-routes-get-fields-endpoint-ip-policy) | ip restriction module configuration or `null` |
| `circuit_breaker` | [EndpointCircuitBreaker](#api-edges-https-routes-get-fields-endpoint-circuit-breaker) | circuit breaker module configuration or `null` |
| `compression` | [EndpointCompression](#api-edges-https-routes-get-fields-endpoint-compression) | compression module configuration or `null` |
| `request_headers` | [EndpointRequestHeaders](#api-edges-https-routes-get-fields-endpoint-request-headers) | request headers module configuration or `null` |
| `response_headers` | [EndpointResponseHeaders](#api-edges-https-routes-get-fields-endpoint-response-headers) | response headers module configuration or `null` |
| `webhook_verification` | [EndpointWebhookValidation](#api-edges-https-routes-get-fields-endpoint-webhook-validation) | webhook verification module configuration or `null` |
| `oauth` | [EndpointOAuth](#api-edges-https-routes-get-fields-endpoint-o-auth) | oauth module configuration or `null` |
| `saml` | [EndpointSAML](#api-edges-https-routes-get-fields-endpoint-saml) | saml module configuration or `null` |
| `oidc` | [EndpointOIDC](#api-edges-https-routes-get-fields-endpoint-oidc) | oidc module configuration or `null` |
| `websocket_tcp_converter` | [EndpointWebsocketTCPConverter](#api-edges-https-routes-get-fields-endpoint-websocket-tcp-converter) | websocket to tcp adapter configuration or `null` |

#### EndpointBackend fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `backend` | [Ref](#api-edges-https-routes-get-fields-ref) | backend to be used to back this endpoint |

#### Ref fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `id` | string | a resource identifier |
| `uri` | string | a uri for locating a resource |

#### EndpointIPPolicy fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `ip_policies` | [Ref](#api-edges-https-routes-get-fields-ref) |  |

#### EndpointCircuitBreaker fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `tripped_duration` | uint32 | Integer number of seconds after which the circuit is tripped to wait before re-evaluating upstream health |
| `rolling_window` | uint32 | Integer number of seconds in the statistical rolling window that metrics are retained for. |
| `num_buckets` | uint32 | Integer number of buckets into which metrics are retained. Max 128. |
| `volume_threshold` | uint32 | Integer number of requests in a rolling window that will trip the circuit. Helpful if traffic volume is low. |
| `error_threshold_percentage` | float64 | Error threshold percentage should be between 0 - 1.0, not 0-100.0 |

#### EndpointCompression fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |

#### EndpointRequestHeaders fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `add` | Map&lt;string, string&gt; | a map of header key to header value that will be injected into the HTTP Request before being sent to the upstream application server |
| `remove` | List&lt;string&gt; | a list of header names that will be removed from the HTTP Request before being sent to the upstream application server |

#### EndpointResponseHeaders fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `add` | Map&lt;string, string&gt; | a map of header key to header value that will be injected into the HTTP Response returned to the HTTP client |
| `remove` | List&lt;string&gt; | a list of header names that will be removed from the HTTP Response returned to the HTTP client |

#### EndpointWebhookValidation fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `provider` | string | a string indicating which webhook provider will be sending webhooks to this endpoint. Value must be one of the supported providers defined at https://ngrok.com/docs/cloud-edge#webhook-verification |
| `secret` | string | a string secret used to validate requests from the given provider. All providers except AWS SNS require a secret |

#### EndpointOAuth fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `provider` | [EndpointOAuthProvider](#api-edges-https-routes-get-fields-endpoint-o-auth-provider) | an object which defines the identity provider to use for authentication and configuration for who may access the endpoint |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `auth_check_interval` | uint32 | Integer number of seconds after which ngrok guarantees it will refresh user state from the identity provider and recheck whether the user is still authorized to access the endpoint. This is the preferred tunable to use to enforce a minimum amount of time after which a revoked user will no longer be able to access the resource. |

#### EndpointOAuthProvider fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `github` | [EndpointOAuthGitHub](#api-edges-https-routes-get-fields-endpoint-o-auth-git-hub) | configuration for using github as the identity provider |
| `facebook` | [EndpointOAuthFacebook](#api-edges-https-routes-get-fields-endpoint-o-auth-facebook) | configuration for using facebook as the identity provider |
| `microsoft` | [EndpointOAuthMicrosoft](#api-edges-https-routes-get-fields-endpoint-o-auth-microsoft) | configuration for using microsoft as the identity provider |
| `google` | [EndpointOAuthGoogle](#api-edges-https-routes-get-fields-endpoint-o-auth-google) | configuration for using google as the identity provider |
| `linkedin` | [EndpointOAuthLinkedIn](#api-edges-https-routes-get-fields-endpoint-o-auth-linked-in) | configuration for using linkedin as the identity provider |
| `gitlab` | [EndpointOAuthGitLab](#api-edges-https-routes-get-fields-endpoint-o-auth-git-lab) | configuration for using gitlab as the identity provider |

#### EndpointOAuthGitHub fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |
| `teams` | List&lt;string&gt; | a list of github teams identifiers. users will be allowed access to the endpoint if they are a member of any of these teams. identifiers should be in the 'slug' format qualified with the org name, e.g. `org-name/team-name` |
| `organizations` | List&lt;string&gt; | a list of github org identifiers. users who are members of any of the listed organizations will be allowed access. identifiers should be the organization's 'slug' |

#### EndpointOAuthFacebook fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthMicrosoft fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthGoogle fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthLinkedIn fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string |  |
| `client_secret` | string |  |
| `scopes` | List&lt;string&gt; |  |
| `email_addresses` | List&lt;string&gt; |  |
| `email_domains` | List&lt;string&gt; |  |

#### EndpointOAuthGitLab fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string |  |
| `client_secret` | string |  |
| `scopes` | List&lt;string&gt; |  |
| `email_addresses` | List&lt;string&gt; |  |
| `email_domains` | List&lt;string&gt; |  |

#### EndpointSAML fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `idp_metadata` | string | The full XML IdP EntityDescriptor. Your IdP may provide this to you as a a file to download or as a URL. |
| `force_authn` | boolean | If true, indicates that whenever we redirect a user to the IdP for authentication that the IdP must prompt the user for authentication credentials even if the user already has a valid session with the IdP. |
| `allow_idp_initiated` | boolean | If true, the IdP may initiate a login directly (e.g. the user does not need to visit the endpoint first and then be redirected). The IdP should set the `RelayState` parameter to the target URL of the resource they want the user to be redirected to after the SAML login assertion has been processed. |
| `authorized_groups` | List&lt;string&gt; | If present, only users who are a member of one of the listed groups may access the target endpoint. |
| `entity_id` | string | The SP Entity's unique ID. This always takes the form of a URL. In ngrok's implementation, this URL is the same as the metadata URL. This will need to be specified to the IdP as configuration. |
| `assertion_consumer_service_url` | string | The public URL of the SP's Assertion Consumer Service. This is where the IdP will redirect to during an authentication flow. This will need to be specified to the IdP as configuration. |
| `single_logout_url` | string | The public URL of the SP's Single Logout Service. This is where the IdP will redirect to during a single logout flow. This will optionally need to be specified to the IdP as configuration. |
| `request_signing_certificate_pem` | string | PEM-encoded x.509 certificate of the key pair that is used to sign all SAML requests that the ngrok SP makes to the IdP. Many IdPs do not support request signing verification, but we highly recommend specifying this in the IdP's configuration if it is supported. |
| `metadata_url` | string | A public URL where the SP's metadata is hosted. If an IdP supports dynamic configuration, this is the URL it can use to retrieve the SP metadata. |
| `nameid_format` | string | Defines the name identifier format the SP expects the IdP to use in its assertions to identify subjects. If unspecified, a default value of `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent` will be used. A subset of the allowed values enumerated by the SAML specification are supported. |

#### EndpointOIDC fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `issuer` | string | URL of the OIDC "OpenID provider". This is the base URL used for discovery. |
| `client_id` | string | The OIDC app's client ID and OIDC audience. |
| `client_secret` | string | The OIDC app's client secret. |
| `scopes` | List&lt;string&gt; | The set of scopes to request from the OIDC identity provider. |

#### EndpointWebsocketTCPConverter fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |


## Update HTTPS Edge Route
Updates an HTTPS Edge Route by ID. If a module is not specified in the update, it will not be modified. However, each module configuration that is specified will completely replace the existing value. There is no way to delete an existing module via this API, instead use the delete module API.

### Request


PATCH /edges/https/{edge_id}/routes/{id}

<EdgesHTTPSRoutesUpdateRequest />


#### Parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `edge_id` | string | unique identifier of this edge |
| `id` | string | unique identifier of this edge route |
| `match_type` | string | Type of match to use for this route. Valid values are "exact_path" and "path_prefix". |
| `match` | string | Route selector: "/blog" or "example.com" or "example.com/blog" |
| `description` | string | human-readable description of what this edge will be used for; optional, max 255 bytes. |
| `metadata` | string | arbitrary user-defined machine-readable data of this edge. Optional, max 4096 bytes. |
| `backend` | [EndpointBackendMutate](#api-edges-https-routes-update-parameters-endpoint-backend-mutate) | backend module configuration or `null` |
| `ip_restriction` | [EndpointIPPolicyMutate](#api-edges-https-routes-update-parameters-endpoint-ip-policy-mutate) | ip restriction module configuration or `null` |
| `circuit_breaker` | [EndpointCircuitBreaker](#api-edges-https-routes-update-parameters-endpoint-circuit-breaker) | circuit breaker module configuration or `null` |
| `compression` | [EndpointCompression](#api-edges-https-routes-update-parameters-endpoint-compression) | compression module configuration or `null` |
| `request_headers` | [EndpointRequestHeaders](#api-edges-https-routes-update-parameters-endpoint-request-headers) | request headers module configuration or `null` |
| `response_headers` | [EndpointResponseHeaders](#api-edges-https-routes-update-parameters-endpoint-response-headers) | response headers module configuration or `null` |
| `webhook_verification` | [EndpointWebhookValidation](#api-edges-https-routes-update-parameters-endpoint-webhook-validation) | webhook verification module configuration or `null` |
| `oauth` | [EndpointOAuth](#api-edges-https-routes-update-parameters-endpoint-o-auth) | oauth module configuration or `null` |
| `saml` | [EndpointSAMLMutate](#api-edges-https-routes-update-parameters-endpoint-saml-mutate) | saml module configuration or `null` |
| `oidc` | [EndpointOIDC](#api-edges-https-routes-update-parameters-endpoint-oidc) | oidc module configuration or `null` |
| `websocket_tcp_converter` | [EndpointWebsocketTCPConverter](#api-edges-https-routes-update-parameters-endpoint-websocket-tcp-converter) | websocket to tcp adapter configuration or `null` |

#### EndpointBackendMutate parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `backend_id` | string | backend to be used to back this endpoint |

#### EndpointIPPolicyMutate parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `ip_policy_ids` | List&lt;string&gt; | list of all IP policies that will be used to check if a source IP is allowed access to the endpoint |

#### EndpointCircuitBreaker parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `tripped_duration` | uint32 | Integer number of seconds after which the circuit is tripped to wait before re-evaluating upstream health |
| `rolling_window` | uint32 | Integer number of seconds in the statistical rolling window that metrics are retained for. |
| `num_buckets` | uint32 | Integer number of buckets into which metrics are retained. Max 128. |
| `volume_threshold` | uint32 | Integer number of requests in a rolling window that will trip the circuit. Helpful if traffic volume is low. |
| `error_threshold_percentage` | float64 | Error threshold percentage should be between 0 - 1.0, not 0-100.0 |

#### EndpointCompression parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |

#### EndpointRequestHeaders parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `add` | Map&lt;string, string&gt; | a map of header key to header value that will be injected into the HTTP Request before being sent to the upstream application server |
| `remove` | List&lt;string&gt; | a list of header names that will be removed from the HTTP Request before being sent to the upstream application server |

#### EndpointResponseHeaders parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `add` | Map&lt;string, string&gt; | a map of header key to header value that will be injected into the HTTP Response returned to the HTTP client |
| `remove` | List&lt;string&gt; | a list of header names that will be removed from the HTTP Response returned to the HTTP client |

#### EndpointWebhookValidation parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `provider` | string | a string indicating which webhook provider will be sending webhooks to this endpoint. Value must be one of the supported providers defined at https://ngrok.com/docs/cloud-edge#webhook-verification |
| `secret` | string | a string secret used to validate requests from the given provider. All providers except AWS SNS require a secret |

#### EndpointOAuth parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `provider` | [EndpointOAuthProvider](#api-edges-https-routes-update-parameters-endpoint-o-auth-provider) | an object which defines the identity provider to use for authentication and configuration for who may access the endpoint |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `auth_check_interval` | uint32 | Integer number of seconds after which ngrok guarantees it will refresh user state from the identity provider and recheck whether the user is still authorized to access the endpoint. This is the preferred tunable to use to enforce a minimum amount of time after which a revoked user will no longer be able to access the resource. |

#### EndpointOAuthProvider parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `github` | [EndpointOAuthGitHub](#api-edges-https-routes-update-parameters-endpoint-o-auth-git-hub) | configuration for using github as the identity provider |
| `facebook` | [EndpointOAuthFacebook](#api-edges-https-routes-update-parameters-endpoint-o-auth-facebook) | configuration for using facebook as the identity provider |
| `microsoft` | [EndpointOAuthMicrosoft](#api-edges-https-routes-update-parameters-endpoint-o-auth-microsoft) | configuration for using microsoft as the identity provider |
| `google` | [EndpointOAuthGoogle](#api-edges-https-routes-update-parameters-endpoint-o-auth-google) | configuration for using google as the identity provider |
| `linkedin` | [EndpointOAuthLinkedIn](#api-edges-https-routes-update-parameters-endpoint-o-auth-linked-in) | configuration for using linkedin as the identity provider |
| `gitlab` | [EndpointOAuthGitLab](#api-edges-https-routes-update-parameters-endpoint-o-auth-git-lab) | configuration for using gitlab as the identity provider |

#### EndpointOAuthGitHub parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |
| `teams` | List&lt;string&gt; | a list of github teams identifiers. users will be allowed access to the endpoint if they are a member of any of these teams. identifiers should be in the 'slug' format qualified with the org name, e.g. `org-name/team-name` |
| `organizations` | List&lt;string&gt; | a list of github org identifiers. users who are members of any of the listed organizations will be allowed access. identifiers should be the organization's 'slug' |

#### EndpointOAuthFacebook parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthMicrosoft parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthGoogle parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthLinkedIn parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string |  |
| `client_secret` | string |  |
| `scopes` | List&lt;string&gt; |  |
| `email_addresses` | List&lt;string&gt; |  |
| `email_domains` | List&lt;string&gt; |  |

#### EndpointOAuthGitLab parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string |  |
| `client_secret` | string |  |
| `scopes` | List&lt;string&gt; |  |
| `email_addresses` | List&lt;string&gt; |  |
| `email_domains` | List&lt;string&gt; |  |

#### EndpointSAMLMutate parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `idp_metadata` | string | The full XML IdP EntityDescriptor. Your IdP may provide this to you as a a file to download or as a URL. |
| `force_authn` | boolean | If true, indicates that whenever we redirect a user to the IdP for authentication that the IdP must prompt the user for authentication credentials even if the user already has a valid session with the IdP. |
| `allow_idp_initiated` | boolean | If true, the IdP may initiate a login directly (e.g. the user does not need to visit the endpoint first and then be redirected). The IdP should set the `RelayState` parameter to the target URL of the resource they want the user to be redirected to after the SAML login assertion has been processed. |
| `authorized_groups` | List&lt;string&gt; | If present, only users who are a member of one of the listed groups may access the target endpoint. |
| `nameid_format` | string | Defines the name identifier format the SP expects the IdP to use in its assertions to identify subjects. If unspecified, a default value of `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent` will be used. A subset of the allowed values enumerated by the SAML specification are supported. |

#### EndpointOIDC parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `issuer` | string | URL of the OIDC "OpenID provider". This is the base URL used for discovery. |
| `client_id` | string | The OIDC app's client ID and OIDC audience. |
| `client_secret` | string | The OIDC app's client secret. |
| `scopes` | List&lt;string&gt; | The set of scopes to request from the OIDC identity provider. |

#### EndpointWebsocketTCPConverter parameters


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |


### Response


Returns a 200 response  on success

<EdgesHTTPSRoutesUpdateResponse />


#### Fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `edge_id` | string | unique identifier of this edge |
| `id` | string | unique identifier of this edge route |
| `created_at` | string | timestamp when the edge configuration was created, RFC 3339 format |
| `match_type` | string | Type of match to use for this route. Valid values are "exact_path" and "path_prefix". |
| `match` | string | Route selector: "/blog" or "example.com" or "example.com/blog" |
| `uri` | string | URI of the edge API resource |
| `description` | string | human-readable description of what this edge will be used for; optional, max 255 bytes. |
| `metadata` | string | arbitrary user-defined machine-readable data of this edge. Optional, max 4096 bytes. |
| `backend` | [EndpointBackend](#api-edges-https-routes-update-fields-endpoint-backend) | backend module configuration or `null` |
| `ip_restriction` | [EndpointIPPolicy](#api-edges-https-routes-update-fields-endpoint-ip-policy) | ip restriction module configuration or `null` |
| `circuit_breaker` | [EndpointCircuitBreaker](#api-edges-https-routes-update-fields-endpoint-circuit-breaker) | circuit breaker module configuration or `null` |
| `compression` | [EndpointCompression](#api-edges-https-routes-update-fields-endpoint-compression) | compression module configuration or `null` |
| `request_headers` | [EndpointRequestHeaders](#api-edges-https-routes-update-fields-endpoint-request-headers) | request headers module configuration or `null` |
| `response_headers` | [EndpointResponseHeaders](#api-edges-https-routes-update-fields-endpoint-response-headers) | response headers module configuration or `null` |
| `webhook_verification` | [EndpointWebhookValidation](#api-edges-https-routes-update-fields-endpoint-webhook-validation) | webhook verification module configuration or `null` |
| `oauth` | [EndpointOAuth](#api-edges-https-routes-update-fields-endpoint-o-auth) | oauth module configuration or `null` |
| `saml` | [EndpointSAML](#api-edges-https-routes-update-fields-endpoint-saml) | saml module configuration or `null` |
| `oidc` | [EndpointOIDC](#api-edges-https-routes-update-fields-endpoint-oidc) | oidc module configuration or `null` |
| `websocket_tcp_converter` | [EndpointWebsocketTCPConverter](#api-edges-https-routes-update-fields-endpoint-websocket-tcp-converter) | websocket to tcp adapter configuration or `null` |

#### EndpointBackend fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `backend` | [Ref](#api-edges-https-routes-update-fields-ref) | backend to be used to back this endpoint |

#### Ref fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `id` | string | a resource identifier |
| `uri` | string | a uri for locating a resource |

#### EndpointIPPolicy fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `ip_policies` | [Ref](#api-edges-https-routes-update-fields-ref) |  |

#### EndpointCircuitBreaker fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `tripped_duration` | uint32 | Integer number of seconds after which the circuit is tripped to wait before re-evaluating upstream health |
| `rolling_window` | uint32 | Integer number of seconds in the statistical rolling window that metrics are retained for. |
| `num_buckets` | uint32 | Integer number of buckets into which metrics are retained. Max 128. |
| `volume_threshold` | uint32 | Integer number of requests in a rolling window that will trip the circuit. Helpful if traffic volume is low. |
| `error_threshold_percentage` | float64 | Error threshold percentage should be between 0 - 1.0, not 0-100.0 |

#### EndpointCompression fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |

#### EndpointRequestHeaders fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `add` | Map&lt;string, string&gt; | a map of header key to header value that will be injected into the HTTP Request before being sent to the upstream application server |
| `remove` | List&lt;string&gt; | a list of header names that will be removed from the HTTP Request before being sent to the upstream application server |

#### EndpointResponseHeaders fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `add` | Map&lt;string, string&gt; | a map of header key to header value that will be injected into the HTTP Response returned to the HTTP client |
| `remove` | List&lt;string&gt; | a list of header names that will be removed from the HTTP Response returned to the HTTP client |

#### EndpointWebhookValidation fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `provider` | string | a string indicating which webhook provider will be sending webhooks to this endpoint. Value must be one of the supported providers defined at https://ngrok.com/docs/cloud-edge#webhook-verification |
| `secret` | string | a string secret used to validate requests from the given provider. All providers except AWS SNS require a secret |

#### EndpointOAuth fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `provider` | [EndpointOAuthProvider](#api-edges-https-routes-update-fields-endpoint-o-auth-provider) | an object which defines the identity provider to use for authentication and configuration for who may access the endpoint |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `auth_check_interval` | uint32 | Integer number of seconds after which ngrok guarantees it will refresh user state from the identity provider and recheck whether the user is still authorized to access the endpoint. This is the preferred tunable to use to enforce a minimum amount of time after which a revoked user will no longer be able to access the resource. |

#### EndpointOAuthProvider fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `github` | [EndpointOAuthGitHub](#api-edges-https-routes-update-fields-endpoint-o-auth-git-hub) | configuration for using github as the identity provider |
| `facebook` | [EndpointOAuthFacebook](#api-edges-https-routes-update-fields-endpoint-o-auth-facebook) | configuration for using facebook as the identity provider |
| `microsoft` | [EndpointOAuthMicrosoft](#api-edges-https-routes-update-fields-endpoint-o-auth-microsoft) | configuration for using microsoft as the identity provider |
| `google` | [EndpointOAuthGoogle](#api-edges-https-routes-update-fields-endpoint-o-auth-google) | configuration for using google as the identity provider |
| `linkedin` | [EndpointOAuthLinkedIn](#api-edges-https-routes-update-fields-endpoint-o-auth-linked-in) | configuration for using linkedin as the identity provider |
| `gitlab` | [EndpointOAuthGitLab](#api-edges-https-routes-update-fields-endpoint-o-auth-git-lab) | configuration for using gitlab as the identity provider |

#### EndpointOAuthGitHub fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |
| `teams` | List&lt;string&gt; | a list of github teams identifiers. users will be allowed access to the endpoint if they are a member of any of these teams. identifiers should be in the 'slug' format qualified with the org name, e.g. `org-name/team-name` |
| `organizations` | List&lt;string&gt; | a list of github org identifiers. users who are members of any of the listed organizations will be allowed access. identifiers should be the organization's 'slug' |

#### EndpointOAuthFacebook fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthMicrosoft fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthGoogle fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string | the OAuth app client ID. retrieve it from the identity provider's dashboard where you created your own OAuth app. optional. if unspecified, ngrok will use its own managed oauth application which has additional restrictions. see the OAuth module docs for more details. if present, client_secret must be present as well. |
| `client_secret` | string | the OAuth app client secret. retrieve if from the identity provider's dashboard where you created your own OAuth app. optional, see all of the caveats in the docs for `client_id`. |
| `scopes` | List&lt;string&gt; | a list of provider-specific OAuth scopes with the permissions your OAuth app would like to ask for. these may not be set if you are using the ngrok-managed oauth app (i.e. you must pass both `client_id` and `client_secret` to set scopes) |
| `email_addresses` | List&lt;string&gt; | a list of email addresses of users authenticated by identity provider who are allowed access to the endpoint |
| `email_domains` | List&lt;string&gt; | a list of email domains of users authenticated by identity provider who are allowed access to the endpoint |

#### EndpointOAuthLinkedIn fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string |  |
| `client_secret` | string |  |
| `scopes` | List&lt;string&gt; |  |
| `email_addresses` | List&lt;string&gt; |  |
| `email_domains` | List&lt;string&gt; |  |

#### EndpointOAuthGitLab fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `client_id` | string |  |
| `client_secret` | string |  |
| `scopes` | List&lt;string&gt; |  |
| `email_addresses` | List&lt;string&gt; |  |
| `email_domains` | List&lt;string&gt; |  |

#### EndpointSAML fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `idp_metadata` | string | The full XML IdP EntityDescriptor. Your IdP may provide this to you as a a file to download or as a URL. |
| `force_authn` | boolean | If true, indicates that whenever we redirect a user to the IdP for authentication that the IdP must prompt the user for authentication credentials even if the user already has a valid session with the IdP. |
| `allow_idp_initiated` | boolean | If true, the IdP may initiate a login directly (e.g. the user does not need to visit the endpoint first and then be redirected). The IdP should set the `RelayState` parameter to the target URL of the resource they want the user to be redirected to after the SAML login assertion has been processed. |
| `authorized_groups` | List&lt;string&gt; | If present, only users who are a member of one of the listed groups may access the target endpoint. |
| `entity_id` | string | The SP Entity's unique ID. This always takes the form of a URL. In ngrok's implementation, this URL is the same as the metadata URL. This will need to be specified to the IdP as configuration. |
| `assertion_consumer_service_url` | string | The public URL of the SP's Assertion Consumer Service. This is where the IdP will redirect to during an authentication flow. This will need to be specified to the IdP as configuration. |
| `single_logout_url` | string | The public URL of the SP's Single Logout Service. This is where the IdP will redirect to during a single logout flow. This will optionally need to be specified to the IdP as configuration. |
| `request_signing_certificate_pem` | string | PEM-encoded x.509 certificate of the key pair that is used to sign all SAML requests that the ngrok SP makes to the IdP. Many IdPs do not support request signing verification, but we highly recommend specifying this in the IdP's configuration if it is supported. |
| `metadata_url` | string | A public URL where the SP's metadata is hosted. If an IdP supports dynamic configuration, this is the URL it can use to retrieve the SP metadata. |
| `nameid_format` | string | Defines the name identifier format the SP expects the IdP to use in its assertions to identify subjects. If unspecified, a default value of `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent` will be used. A subset of the allowed values enumerated by the SAML specification are supported. |

#### EndpointOIDC fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |
| `options_passthrough` | boolean | Do not enforce authentication on HTTP OPTIONS requests. necessary if you are supporting CORS. |
| `cookie_prefix` | string | the prefix of the session cookie that ngrok sets on the http client to cache authentication. default is 'ngrok.' |
| `inactivity_timeout` | uint32 | Integer number of seconds of inactivity after which if the user has not accessed the endpoint, their session will time out and they will be forced to reauthenticate. |
| `maximum_duration` | uint32 | Integer number of seconds of the maximum duration of an authenticated session. After this period is exceeded, a user must reauthenticate. |
| `issuer` | string | URL of the OIDC "OpenID provider". This is the base URL used for discovery. |
| `client_id` | string | The OIDC app's client ID and OIDC audience. |
| `client_secret` | string | The OIDC app's client secret. |
| `scopes` | List&lt;string&gt; | The set of scopes to request from the OIDC identity provider. |

#### EndpointWebsocketTCPConverter fields


|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `enabled` | boolean | `true` if the module will be applied to traffic, `false` to disable. default `true` if unspecified |


## Delete HTTPS Edge Route
Delete an HTTPS Edge Route by ID

### Request


DELETE /edges/https/{edge_id}/routes/{id}

<EdgesHTTPSRoutesDeleteRequest />


### Response


Returns a 204 response with no body on success