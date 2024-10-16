# This is a FAQ

## About OOAPI

- [How to connect OOAPI endpoints?](./connecting-ooapi.endpoints.md)
- Where can I find an example of protecting my API (`/persons/` `/assiciations/`) using oauth/MyAcademicID?
  - [Mock implementation of a home institution](https://github.com/SURFnet/student-mobility-home-institution-mock)

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
  and send it to Peter or Jelmer:

    ``` yaml
    schacHome: demoinst02.eduxchange.eu
    name: Demo 02 home  # The display name for your institution
    abbreviation: Demo02home  # The short name
    courseEndpoint: "https://demoinst02.eduxchange.eu/basic/offerings"  # The OOAPI endpoint for courses
    personsEndpoint: "https://demoinst02.eduxchange.eu/persons/me"  # The OOAPI endpoint for persons
    personAuthentication: HEADER  # Should always be HEADER for token based authentication
    associationsEndpoint: "https://demoinst02.eduxchange.eu/associations" # The OOAPI endpoint for courses
    authenticationEndpoint: "https://demoinst01.eduxchange.eu/api/enrollment" # This is on the enrollment-receiver
    registrationEndpoint: "https://demoinst01.eduxchange.eu/api/start"  # This is on the enrollment-receiver
    registrationUser: "user"  # These are configured in the broker section of the enrollment-receiver config file
    registrationPassword: "secret"  # These are configured in the broker section of the enrollment-receiver config file
    logoURI: https://static.surfconext.nl/logos/idp/surf.png # The logo to be displayed for your institution
    scopes: demoinst02.eduxchange.eu/persons demoinst02.eduxchange.eu/results   # The scopes required for accessing the persons and associations endpoints
    privacyEndpoint: https://www.surf.nl/studentmobiliteit  # A link to the privacy declaration
    useEduHubForOffering: True # Should always be True for EuroTeq
    useQueueIt: False # Should always be False for EuroTeq
    ```

- What are scopes? What should I use for scopes?
- How do I connect to MyAcademicID?
  - The enrollment receiver needs to be connected as a relying party to
  MyAcademicID. The API server needs to be connected as a Resource Server.
  Both can be registered by [filling in the myacademic id registration form](./registration.md)
