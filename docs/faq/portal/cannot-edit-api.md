# (API Manager Portal) As a publisher, I am not able to edit the API

An API in a published state cannot be modified, we recommend publishers to create a new API to upgrade the original API to it. And if publishers was to unpublish the API to make any changes to it, this will cause the API to lost the API to APP relationship. This relationship can only be established again with the help of the consumer, without the API to APP linkage being re-establish API calls which requires inbound authentication on APEX Cloud will fail.

Therefore publisher are advice to use the [Update API](docs/publisher/update-api.md) workflow to make any changes to their API.