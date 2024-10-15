# connectngo-php-test

You are given a PHP Laravel service named `JiraService`. This service is responsible for creating, updating, fetching, deleting, and searching JIRA issues via the JIRA REST API. 
The service, as it stands, contains several intentional mistakes and opportunities for improvement.

Your task is to:

- Identify the mistakes and potential improvements.
- Refactor the code to address these issues.
- Provide explanations for the changes you made.
    
```php
<?php

namespace App\Services;

use GuzzleHttp\Client;

class JiraService
{
    protected $client;
    protected $baseUrl;
    protected $username;
    protected $apiToken;

    public function __construct()
    {
        $this->client = new Client();
        $this->baseUrl = config('app.jira_url');
        $this->username = config('app.jira_username');
        $this->apiToken = config('app.jira_api_token');
    }

    public function createIssue(array $data)
    {
        $url = $this->baseUrl . '/rest/api/2/issue';
        $response = $this->client->post($url, [
            'auth' => [$this->username, $this->apiToken],
            'json' => $data
        ]);

        return json_decode($response->getBody()->getContents(), true);
    }

    public function updateIssue($issueId, array $data)
    {
        $url = $this->baseUrl . '/rest/api/2/issue/' . $issueId;
        $response = $this->client->put($url, [
            'auth' => [$this->username, $this->apiToken],
            'json' => $data
        ]);

        return json_decode($response->getBody()->getContents(), true);
    }

    public function getIssue($issueId)
    {
        $url = $this->baseUrl . '/rest/api/2/issue/' . $issueId;
        $response = $this->client->get($url, [
            'auth' => [$this->username, $this->apiToken]
        ]);

        return json_decode($response->getBody()->getContents(), true);
    }

    public function deleteIssue($issueId)
    {
        $url = $this->baseUrl . '/rest/api/2/issue/' . $issueId;
        $response = $this->client->delete($url, [
            'auth' => [$this->username, $this->apiToken]
        ]);

        return json_decode($response->getBody()->getContents(), true);
    }

    public function searchIssues($jql)
    {
        $url = $this->baseUrl . '/rest/api/2/search?jql=' . urlencode($jql);
        $response = $this->client->get($url, [
            'auth' => [$this->username, $this->apiToken]
        ]);

        return json_decode($response->getBody()->getContents(), true);
    }
}

```
