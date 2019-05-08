<template>
  <div class="rideshare">
    <div class="container">
      <div class="row">
        <div class="col-xl-4 col-lg-5">
          <div class="card card-primary animated fadeInUp animation-delay-7">
            <div class="card-body">
              <div class="text-center mb-2">
                <span class="ms-logo ms-logo-sm mr-1">M</span>
                <h3 class="no-m ms-site-title">
                  Material
                  <span>Style</span>
                </h3>
              </div>
              <form class="form-horizontal">
                <fieldset class="container">
                  <div class="form-group row">
                    <label for="inputEmail" autocomplete="false" class="col-lg-2 control-label">Name</label>
                    <div class="col-lg-9">
                      <input type="text" class="form-control" id="inputName" placeholder="Name" />
                    </div>
                  </div>
                  <div class="form-group row">
                    <label for="inputEmail" autocomplete="false" class="col-lg-2 control-label">Email</label>
                    <div class="col-lg-9">
                      <input type="email" class="form-control" id="inputEmail" placeholder="Email" />
                    </div>
                  </div>
                  <div class="form-group row">
                    <label for="inputEmail" autocomplete="false" class="col-lg-2 control-label">Subject</label>
                    <div class="col-lg-9">
                      <input type="text" class="form-control" id="inputSubject" placeholder="Subject" />
                    </div>
                  </div>
                  <div class="form-group row">
                    <label for="textArea" class="col-lg-2 control-label">Message</label>
                    <div class="col-lg-9">
                      <textarea class="form-control" rows="3" id="textArea" placeholder="Your message..."></textarea>
                    </div>
                  </div>
                  <div class="form-group row justify-content-end">
                    <div class="col-lg-10">
                      <button type="submit" class="btn btn-raised btn-primary">Quill it!</button>
                      <button type="button" class="btn btn-danger">Cancel</button>
                    </div>
                  </div>
                </fieldset>
              </form>
            </div>
          </div>
        </div>
        <div class="col-xl-8 col-lg-7">
          <div class="card card-primary animated fadeInUp animation-delay-7">
            <div class="ms-hero-bg-primary ms-hero-img-mountain">
              <h2 class="text-center no-m pt-4 pb-4 color-white index-1">Quill Content</h2>
            </div>
            <div class="card-body" id="quill-content">
              <p>
                So, you prefer to stare out the window in private contemplation or peacefully scroll through your phone
                as you’re getting from A to B. We get it.
              </p>
              <p>You’re in good company. 80% of rides departing from Lincoln
                Park are individual (non-pool/shared) fares. The typical individual ride leaving Lincoln Park will on
                average cost around $13. Your desire to ride solo is coming at a cost, however. The average price of a
                pool/shared ride departing from Lincoln Park is $7 - so you’re paying an additional $6 on average to
                avoid that awkward “hello” to a fellow rider or - heaven forbid - having to ride shotgun.</p>
              <p>During the week, individual fares on average are $2 cheaper than the going rate on the weekend. The most expensive time to rideshare from Lincoln Park is Saturday night ($15) and the least expensive time is Tuesday
                afternoon ($8).</p>
                <p>If you’re feeling lucky, 28% of shared rides departing from Lincoln Park do not pick up
                additional passengers along the way. The best time to request a shared ride with the likelihood of
                having no traveling companions is Wednesday evening. However if you do get stuck with additional riders,
                you’ll make an average of 2.2 stops before you’re dropped off at your destination.
              </p>
              <p>Lincoln Park rider aren’t the most generous of tippers. The average tip for an individual fare is $0.46. That’s higher than
                the average tip from shared fare passengers, averaging a paltry $0.17.
                </p>
                <p>Lincoln Park had its fair share of unusual trips. One rider on took a two-hour, forty mile trip only to be dropped off back in Lincoln Park at the end of it. The longest ride to depart from Lincoln Park was 62 miles. The most expensive
                ride from Lincoln Park cost its rider $206. We saw one particularly stingy rider take a one hour fare
                costing $110 and offer no gratuity to their driver. </p>
              </p>
            </div>
          </div>
          <div class="card card-primary animated fadeInUp animation-delay-7" id="google-maps-holder">
            <div class="card-header">
              <h3 class="card-title">
                <i class="zmdi zmdi-map"></i>
                Map
              </h3>
            </div>
            <iframe
              width="100%"
              height="340"
              src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d48342.06480344582!2d-73.980069429762!3d40.775680208459505!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x89c2589a018531e3%3A0xb9df1f7387a94119!2sCentral+Park!5e0!3m2!1sen!2sus!4v1491233314840"
            ></iframe>
          </div>
        </div>
      </div>
    </div>
    <!-- container -->
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  name: "rideshare"
};
</script>
