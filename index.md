# This is a FAQ

## About OOAPI

- [How to connect OOAPI endpoints?](./connecting-ooapi.endpoints.md)

## About Enrolment

- How does the enrolment work in general?
  - [Sequence diagram for EuroTeQ enrolment](./sequence-diagram.md)
- What are the possible initial states of an association?
  - [Flowchart of the association states](./association-states.md)

## About the enrolment receiver

- Can you tell me more about the communication between the generic part of the enrollment
receiver and the custom implementation at the host institution?
  - [Communication dataformat](./dataformat.md)
- Do you have an example configuration of the enrolment receiver?
  - [example application.yaml for enrollment receiver](./application.yaml)
- How do I run the enrollment receiver?
  - It's advised to use the docker image [as described here](./running.md)

## About openID and MyAcedemicID

- What are the url's for starting authentication/introspection/tokens at MyAcedemicID?
  - The MyAcademidID endpoints for openID can be found in the
  [well-known](https://proxy.prod.erasmus.eduteams.org/.well-known/openid-configuration)
  location. Please use a widely used library for all openID parts, instead
  of builing it.
- How can we start an enrollment for testing?
  - There is a [demo enrollment broker](https://demobroker.eduxchange.eu/)
  available for testing.
  - To connect to the demo enrollment broker, edit this yaml document
  and send it to Peter:

    ``` yaml
    schacHome: demoinst02.eduxchange.eu
    name: Demo 02 home
    abbreviation: Demo02home
    courseEndpoint: "https://demoinst02.eduxchange.eu/basic/offerings"
    personsEndpoint: "https://demoinst02.eduxchange.eu/persons/me"
    personAuthentication: HEADER
    associationsEndpoint: "https://demoinst02.eduxchange.eu/associations"
    authenticationEndpoint: "https://demoinst01.eduxchange.eu/api/enrollment"
    registrationEndpoint: "https://demoinst01.eduxchange.eu/api/start"
    registrationUser: "user"
    registrationPassword: "secret"
    logoURI: https://static.surfconext.nl/logos/idp/surf.png
    scopes: demoinst02.eduxchange.eu/persons demoinst02.eduxchange.eu/results
    privacyEndpoint: https://www.surf.nl/studentmobiliteit
    useEduHubForOffering: False
    useQueueIt: False
    ```

- What are scopes? What should I use for scopes?
- How do I connect to MyAcademicID?
  - The enrollment receiver needs to be connected as a relying party to
  MyAcademicID. The API server needs to be connected as a Resource Server.
  Both can be registered by filling in this
  [myacademic id registration form](https://wiki.geant.org/display/SM/Registering+services+on+MyAcademicID)
