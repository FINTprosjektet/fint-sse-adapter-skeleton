# FINT Java SSE Adapter Skeleton
[![FINT javadocs](https://img.shields.io/badge/FINT-javadocs-blue.svg)](https://docs.felleskomponent.no/fint-sse-adapter-skeleton/)

## Introduction

## Packages and files
The adapter is divided into to main packages. The `adapter package` is the core adapter code. In general this don't need
any customization. The `customcode package` (which should be named for example after the application the adapter talks to)
is where the logic of the adapter is placed.

### Action.java
This is a ENUM of all the actions this adapter supports. For example for
the student component the action enum should be something like:

```java
public enum Action {
    GET_ALL_STUDENTS,
    GET_STUDENT,
    UPDATE_STUDENT;

    ...
}

### EventHandlerService.java
The actions is handled in the `handleEvent()` method:

```java
  public void handleEvent(String json) {
   Event event = EventUtil.toEvent(json);
   if (event.isHealthCheck()) {
       postHealthCheckResponse(event);
   } else {
       Event<FintResource> responseEvent = new Event<>(event);
       responseEvent.setStatus(Status.PROVIDER_ACCEPTED);
       eventStatusService.postStatus(responseEvent);

       /*
        * Add if statements for all the actions
        */

       responseEvent.setStatus(Status.PROVIDER_RESPONSE);
       eventResponseService.postResponse(responseEvent);
   }
}
```

## Adapter configuration
| Key | Description | Example |
|-----|-------------|---------|
| fint.adapter.organizations | List of orgIds the adapter handles. | rogfk.no, vaf.no, ofk.no |
| fint.adapter.sse-endpoint | Url to the sse endpoint. | https://api.felleskomponent.no/arbeidstakere/provider/sse |
| fint.adapter.status-endpoint | Url to the status endpoint. | https://api.felleskomponent.no/arbeidstakere/provider/status |
| fint.adapter.response-endpoint | Url to the response endpoint. | https://api.felleskomponent.no/arbeidstakere/provider/response |

