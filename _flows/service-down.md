---
metadata:
  id: "service-down"
  title: "Internal Web App Unavailable"
  description: "Diagnostic flow for when users report a critical internal web application is down."
  type: "Service Unavailable Issues"
steps:
  start:
    title: "Verify Global Impact"
    body: "Check the monitoring dashboard. Is the service down for everyone or just one user?"
    options:
      - label: "Global Outage"
        target: "check_server"
        type: "danger"
      - label: "Single User"
        target: "escalate_tier1"
        type: "primary"
  
  check_server:
    title: "Check Application Servers"
    body: "Log into the server dashboard. Are the nodes healthy?"
    options:
      - label: "Nodes Down"
        target: "restart_nodes"
        type: "primary"
      - label: "Nodes Up, Service Unreachable"
        target: "check_lb"
        type: "primary"

  restart_nodes:
    title: "Restart Application Nodes"
    body: "Perform a rolling restart of the application nodes and verify health status."
    options:
      - label: "Service Restored"
        target: "resolved"
        type: "success"
      - label: "Still Down"
        target: "escalate_eng"
        type: "danger"

  check_lb:
    title: "Check Load Balancer"
    body: "Verify if the load balancer is routing traffic correctly to the application nodes."
    options:
      - label: "LB Misconfigured"
        target: "escalate_eng"
        type: "danger"

  escalate_tier1:
    title: "Standard Desktop Troubleshooting"
    body: "Since only one user is affected, perform standard cache clearing, proxy checks, and restart the browser."
    options: []

  escalate_eng:
    title: "Escalate to Engineering"
    body: "Open an incident and escalate to the Application Engineering team immediately."
    options: []

  resolved:
    title: "Resolved"
    body: "Service has been restored. Update the status page and close the ticket."
    options: []
---
