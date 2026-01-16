In the domain of FundView, the "legacy reskin" is defined as the extraction of services out of the Fast.FundView.MVC.UI project.
By extraction, the static classes will remain in place and delegate the calls to helper services in dual-targeting projects.
The extraction involves: creation of a non-static helper service in a dual-targeting project inside FundViewGit.
The "reskinned" services are functionally identical to the legacy services but with an OOP approach, along with the usage of CancellationToken.
To determine if a service is reskinned, check if the calls are being delegated to a non-static helper service.
There is a separate aspect of the reskin: repositories. Repositories are moved entirely out of the MVC.UI project into dual-targeting projects.
In general, services depend on repositories, so when a service is reskinned, its dependent repositories should be reskinned as well.
