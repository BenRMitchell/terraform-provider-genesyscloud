---
page_title: "genesyscloud_user Resource - terraform-provider-genesyscloud"
subcategory: ""
description: |-
  Genesys Cloud User
---
# genesyscloud_user (Resource)

Genesys Cloud User

## API Usage
The following Genesys Cloud APIs are used by this resource. Ensure your OAuth Client has been granted the necessary scopes and permissions to perform these operations:

* [POST /api/v2/users](https://developer.mypurecloud.com/api/rest/v2/users/#post-api-v2-users)
* [GET /api/v2/users/{userId}](https://developer.mypurecloud.com/api/rest/v2/users/#get-api-v2-users--userId-)
* [PATCH /api/v2/users/{userId}](https://developer.mypurecloud.com/api/rest/v2/users/#patch-api-v2-users--userId-)
* [DELETE /api/v2/users/{userId}](https://developer.mypurecloud.com/api/rest/v2/users/#delete-api-v2-users--userId-)
* [PUT /api/v2/users/{userId}/routingskills/bulk](https://developer.mypurecloud.com/api/rest/v2/users/#put-api-v2-users--userId--routingskills-bulk)
* [DELETE /api/v2/users/{userId}/routinglanguages/{languageId}](https://developer.mypurecloud.com/api/rest/v2/users/#delete-api-v2-users--userId--routinglanguages--languageId-)
* [PATCH /api/v2/users/{userId}/routinglanguages/bulk](https://developer.mypurecloud.com/api/rest/v2/users/#patch-api-v2-users--userId--routinglanguages-bulk)
* [GET /api/v2/users/{userId}/routinglanguages](https://developer.mypurecloud.com/api/rest/v2/users/#get-api-v2-users--userId--routinglanguages)
* [PUT /api/v2/users/{userId}/profileskills](https://developer.mypurecloud.com/api/rest/v2/users/#put-api-v2-users--userId--profileskills)

## Example Usage

```terraform
resource "genesyscloud_user" "test_user" {
  email           = "john@example.com"
  name            = "John Doe"
  password        = "initial-password"
  division_id     = genesyscloud_auth_division.home.id
  state           = "active"
  department      = "Development"
  title           = "Senior Director"
  manager         = genesyscloud_user.test-user-manager.id
  acd_auto_answer = true
  profile_skills  = ["Java", "Go"]
  certifications  = ["Certified Developer"]
  addresses {
    other_emails {
      address = "john@gmail.com"
      type    = "HOME"
    }
    phone_numbers {
      number     = "3174181234"
      media_type = "PHONE"
      type       = "MOBILE"
    }
  }
  routing_skills {
    skill_id    = genesyscloud_routing_skill.test-skill.id
    proficiency = 4.5
  }
  routing_languages {
    skill_id    = genesyscloud_routing_language.english.id
    proficiency = 4
  }
  locations {
    location_id = genesyscloud_location.main-site.id
    notes       = "Office 201"
  }
  employer_info {
    official_name = "Jonathon Doe"
    employee_id   = "12345"
    employee_type = "Full-time"
    date_hire     = "2021-03-18"
  }
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- **email** (String) User's primary email and username.
- **name** (String) User's full name.

### Optional

- **acd_auto_answer** (Boolean) Enable ACD auto-answer. Defaults to `false`.
- **addresses** (List of Object) The address settings for this user. If not set, this resource will not manage addresses. (see [below for nested schema](#nestedatt--addresses))
- **certifications** (Set of String) Certifications for this user. If not set, this resource will not manage certifications.
- **department** (String) User's department.
- **division_id** (String) The division to which this user will belong. If not set, the home division will be used.
- **employer_info** (List of Object) The employer info for this user. If not set, this resource will not manage employer info. (see [below for nested schema](#nestedatt--employer_info))
- **id** (String) The ID of this resource.
- **locations** (Set of Object) The user placement at each site location. If not set, this resource will not manage user locations. (see [below for nested schema](#nestedatt--locations))
- **manager** (String) User ID of this user's manager.
- **password** (String, Sensitive) User's password. If specified, this is only set on user create.
- **profile_skills** (Set of String) Profile skills for this user. If not set, this resource will not manage profile skills.
- **routing_languages** (Block Set) Languages and proficiencies for this user. (see [below for nested schema](#nestedblock--routing_languages))
- **routing_skills** (Block Set) Skills and proficiencies for this user. (see [below for nested schema](#nestedblock--routing_skills))
- **state** (String) User's state (active | inactive). Default is 'active'. Defaults to `active`.
- **title** (String) User's title.

<a id="nestedatt--addresses"></a>
### Nested Schema for `addresses`

Optional:

- **other_emails** (Set of Object) (see [below for nested schema](#nestedobjatt--addresses--other_emails))
- **phone_numbers** (Set of Object) (see [below for nested schema](#nestedobjatt--addresses--phone_numbers))

<a id="nestedobjatt--addresses--other_emails"></a>
### Nested Schema for `addresses.other_emails`

Optional:

- **address** (String)
- **type** (String)


<a id="nestedobjatt--addresses--phone_numbers"></a>
### Nested Schema for `addresses.phone_numbers`

Optional:

- **extension** (String)
- **media_type** (String)
- **number** (String)
- **type** (String)



<a id="nestedatt--employer_info"></a>
### Nested Schema for `employer_info`

Optional:

- **date_hire** (String)
- **employee_id** (String)
- **employee_type** (String)
- **official_name** (String)


<a id="nestedatt--locations"></a>
### Nested Schema for `locations`

Optional:

- **location_id** (String)
- **notes** (String)


<a id="nestedblock--routing_languages"></a>
### Nested Schema for `routing_languages`

Required:

- **language_id** (String) ID of routing language.
- **proficiency** (Number) Proficiency is a rating from 0 to 5 on how competent an agent is for a particular language. It is used when a queue is set to 'Best available language' mode to allow acd interactions to target agents with higher proficiency ratings.


<a id="nestedblock--routing_skills"></a>
### Nested Schema for `routing_skills`

Required:

- **proficiency** (Number) Rating from 0.0 to 5.0 on how competent an agent is for a particular skill. It is used when a queue is set to 'Best available skills' mode to allow acd interactions to target agents with higher proficiency ratings.
- **skill_id** (String) ID of routing skill.
