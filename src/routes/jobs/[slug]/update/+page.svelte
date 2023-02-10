<script>
    import { goto } from '$app/navigation';
	  import { PUBLIC_BACKEND_BASE_URL } from '$env/static/public';
	import { alert } from '../../../../utils/alert';
	  import { getTokenFromLocalStorage, getUserId } from '../../../../utils/auth';
    export let data;
    let formErrors = {};

    function postJob() {
        goto('/');
    }
    console.log(data)
    async function createJob(evt) {
      evt.preventDefault()
      if (evt.target['jobtitle'].value == '') {
        formErrors['jobtitle'] = { message: 'Missing required value.' };
      } else if (evt.target['jobtitle'].value.length < 3 ) {
        formErrors['jobtitle'] = { message: 'Must be at least 3 character(s).' };
      } else if (formErrors.jobtitle) {
        delete formErrors.jobtitle  
        formErrors = formErrors
      }

      if (evt.target['minannualcompensation'].value == ''){
        formErrors['minannualcompensation'] = {message: 'Missing required value'}
      } else if (formErrors.minannualcompensation) {
        delete formErrors.minannualcompensation
        formErrors = formErrors
      }
      
      if (evt.target['maxannualcompensation'].value == ''){
        formErrors['maxannualcompensation'] = {message: 'Missing required value'}
      } else if (formErrors.maxannualcompensation) {
        delete formErrors.maxannualcompensation
        formErrors = formErrors
      }

      if (evt.target['companyname'].value == ''){
        formErrors['companyname'] = {message: 'Missing required value'}
      } else if (formErrors.companyname) {
        delete formErrors.companyname
        formErrors = formErrors
      }

      if (evt.target['joblocation'].value == ''){
        formErrors['joblocation'] = {message: 'Missing required value'}
      } else if (formErrors.joblocation) {
        delete formErrors.joblocation
        formErrors = formErrors
      }

      if (evt.target['description'].value == ''){
        formErrors['description'] = {message: 'Missing required value'}
      } else if (formErrors.description) {
        delete formErrors.description
        formErrors = formErrors
      }

      if (evt.target['requirements'].value == ''){
        formErrors['requirements'] = {message: 'Missing required value'}
      } else if (formErrors.requirements) {
        delete formErrors.requirements
        formErrors = formErrors
      }

      if (evt.target['appinstructions'].value == '') {
        formErrors['appinstructions'] = { message: 'Missing required value.' };
      } else if (evt.target['appinstructions'].value.length < 10 ) {
        formErrors['appinstructions'] = { message: 'Must be at least 10 character(s).' };
      } else if (formErrors.appinstructions) {
        delete formErrors.appinstructions  
        formErrors = formErrors
      }

      if (Object.keys(formErrors).length > 0){
        return
      }

      const userData = {
          user: getUserId(),
          title: evt.target['jobtitle'].value,
          minAnnualCompensation: evt.target['minannualcompensation'].value,
          maxAnnualCompensation: evt.target['maxannualcompensation'].value,
          description: evt.target['description'].value,
          requirements: evt.target['requirements'].value,
          applicationInstructions: evt.target['appinstructions'].value,
          location: evt.target['joblocation'].value,
          employer: evt.target['companyname'].value
      }

      const res = await fetch(PUBLIC_BACKEND_BASE_URL + `/api/collections/jobs/records/${data.job.id}`, {
        method: 'PATCH',
        headers: {
        'Content-Type': 'application/json',
        'Authorization': getTokenFromLocalStorage(),
        'mode': 'cors'
        },
        body: JSON.stringify(userData)
      })

      const resp = await res.json()

      if (res.status == 200){
        postJob()
      } else {
        alert.setAlert(resp.message, "warning")
      }

    }

  </script>
  
  <div class="pl-20 pr-20">
      <form on:submit={createJob} class="w-full">
          <div class="form-control w-full">
              <label class="label" for="jobtitle">
                  <span class="label-text">Job Title</span>
              </label>
              <input
                  type="text"
                  name="jobtitle"
                  placeholder="Software Engineer"
                  value={data.job.title}
                  class="input input-bordered w-full"
              />
              {#if 'jobtitle' in formErrors}
                  <label class="label" for="jobtitle">
                      <span class="label-text-alt text-red-500">{formErrors['jobtitle'].message}</span>
                  </label>
              {/if}
          </div>
  
          <div class="form-control w-full">
              <label class="label" for="minannualcompensation">
                  <span class="label-text">Min Annual Compensation</span>
              </label>
              <input
                  type="number"
                  name="minannualcompensation"
                  placeholder="40000"
                  class="input input-bordered w-full"
                  value={data.job.minAnnualCompensation}
              />
              {#if 'minannualcompensation' in formErrors}
                  <label class="label" for="minannualcompensation">
                      <span class="label-text-alt text-red-500">{formErrors['minannualcompensation'].message}</span>
                  </label>
              {/if}
          </div>

          <div>
            <div class="form-control w-full">
                <label class="label" for="minannualcompensation">
                    <span class="label-text text-xs">USD</span>
                    <span class="label-text text-xs">per annum</span>
          </div>

          <div class="form-control w-full">
            <label class="label" for="maxannualcompensation">
                <span class="label-text">Max Annual Compensation</span>
            </label>
            <input
                type="number"
                name="maxannualcompensation"
                placeholder="250000"
                class="input input-bordered w-full"
                value={data.job.maxAnnualCompensation}
            />
            {#if 'maxannualcompensation' in formErrors}
                <label class="label" for="maxannualcompensation">
                    <span class="label-text-alt text-red-500">{formErrors['maxannualcompensation'].message}</span>
                </label>
            {/if}
        </div>

        <div>
          <div class="form-control w-full">
              <label class="label" for="maxannualcompensation">
                  <span class="label-text text-xs">USD</span>
                  <span class="label-text text-xs">per annum</span>
        </div>

        <div>
            <div class="form-control w-full">
              <label class="label" for="companyname">
                  <span class="label-text">Company Name</span>
              </label>
              <input
                  type="text"
                  name="companyname"
                  placeholder="e.g. Facebook"
                  class="input input-bordered w-full"
                  value={data.job.employer}
              />
              {#if 'companyname' in formErrors}
                  <label class="label" for="jobtitle">
                      <span class="label-text-alt text-red-500">{formErrors['companyname'].message}</span>
                  </label>
              {/if}
        </div>

        <div>
            <div class="form-control w-full">
              <label class="label" for="joblocation">
                  <span class="label-text">Job Location</span>
              </label>
              <input
                  type="text"
                  name="joblocation"
                  placeholder="e.g. Singapore"
                  class="input input-bordered w-full"
                  value={data.job.location}
              />
              {#if 'joblocation' in formErrors}
                  <label class="label" for="joblocation">
                      <span class="label-text-alt text-red-500">{formErrors['joblocation'].message}</span>
                  </label>
              {/if}
        </div>

        <div>
            <div class="form-control w-full">
              <label class="label" for="description">
                  <span class="label-text">Description</span>
              </label>
              <textarea
                  name="description"
                  placeholder=""
                  class="input input-bordered w-full h-60"
                  value={data.job.description}
              />
              {#if 'description' in formErrors}
                  <label class="label" for="description">
                      <span class="label-text-alt text-red-500">{formErrors['description'].message}</span>
                  </label>
              {/if}
        </div>

        <div>
            <div class="form-control w-full">
              <label class="label" for="requirements">
                  <span class="label-text">Requirements</span>
              </label>
              <textarea
                  name="requirements"
                  placeholder=""
                  class="input input-bordered w-full h-60"
                  value={data.job.requirements}
              />
              {#if 'requirements' in formErrors}
                  <label class="label" for="requirements">
                      <span class="label-text-alt text-red-500">{formErrors['requirements'].message}</span>
                  </label>
              {/if}
        </div>

        <div>
            <div class="form-control w-full">
              <label class="label" for="appinstructions">
                  <span class="label-text">Application Instructions</span>
              </label>
              <textarea
                  name="appinstructions"
                  placeholder=""
                  class="input input-bordered w-full h-20"
                  value={data.job.applicationInstructions}
              />
              {#if 'appinstructions' in formErrors}
                  <label class="label" for="appinstructions">
                      <span class="label-text-alt text-red-500">{formErrors['appinstructions'].message}</span>
                  </label>
              {/if}
        </div>

          <div class="form-control w-full mt-8 mb-20">
              <button class="btn btn-md">UPDATE</button>
          </div>
          
      </form>
  </div>