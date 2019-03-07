# git-ddb-docs
GitHub Distributed Database working and implementation notes

### Problem statement
Static websites usually suffers from lack of personalized database i.e. the site behaves similarly for everyony. But what if a static webmaster
wants to provide a personalized experience to it's users. In this case we think of two scenerios
<!--  -->
- Save user preference on free database store such as github or<br>
- Use cloud services like azure, aws etc.
<!--  -->
For the former there are limits to the data which can be stored so not good for website with large number of users and for the latter it's costly to pay for cloud for static a website.

### git-ddb
Git ddb is an database solution primarily for static websites.
The idea behind is to use users database quota for saving their personalized data instead of placing it in one place (repository limits) or cloud database (cost). The data per user is saved in their private repository so it eliminates all the above problems.
<!--  -->
It works the following way:
1. Authorize user with github using OAuth => catch `access_token` to write data to their repository
2. Create a private repository (if not already exists) named `git-ddb`
3. Create a file (if not already exist) distinguishing your site from all other sites using same scheme. example: I save my user's settings in file named _git-ddb-ankurparihar.json_
4. Save user settings in this file whenever there is a change using [git api](https://developer.github.com/v3/) and `access_token`
5. That's all. Every time user log's in use the settings.

### Pros
- Distributed database: Eliminates the restriction of free databases' quota limit
- Free database: No cost for servers
- Transparency: User knows which site is using what configuration
- Customizability: User can change their data anytime from their github accound without even visiting respective webiste
- Privacy: Due to the `git-ddb` being private only the user has access to their data
- Backup and restore: User can backup and restore settings any time 
- Clean: All site using one place reduces mess

### Cons
- A bit slow: fetching data from github takes time around 0.8 seconds. **Workaround** : use cookies
- Request limits: Git api has rate limit of around [5000 requests per hour](https://developer.github.com/v3/#rate-limiting). But it's not actually a practical problem as no site is expected to change settings every second and must not do it
- file size limit: [100 MB](https://help.github.com/en/articles/what-is-my-disk-quota) File limit. [1 MB](https://developer.github.com/v3/repos/contents/#get-contents) Transfer limit. Also here optimize use of resources required
- Server requirement: You need one server to process git oauth request and to hide and store git oauth app's secret tokens
- Not a fair use of github actually. Smells like torrent ;)

### Cautions
- Use unique file names and structure. Use [sitelist](./SiteList.md).
- Always keep repository private
- Do not store sensitive informations without encoding
- Keep your [oauth app](https://developer.github.com/apps/building-oauth-apps/)'s secret keys private
- Respect user privacy
- Take care of [cookie policies](https://www.cookielaw.org/the-cookie-law/) if using any. [workaround](https://cookieconsent.insites.com/)
- For users: make sure to backup regularly

### Resources
- [Git API](https://developer.github.com/v3/)
- Example Implementation: [Git API calls](https://github.com/ankurparihar/ankurparihar.github.io/blob/master/media/personalization.js), [OAuth calls](https://github.com/ankurparihar/ankurparihar.github.io/tree/master/auth)
- [Sites Using git-ddb](./SiteList.md)