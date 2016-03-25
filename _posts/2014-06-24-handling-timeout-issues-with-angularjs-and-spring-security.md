---
layout: post
title:  "Handling timeout issues with AngularJS and Spring Security"
date:   2014-06-24 09:00:13
categories: Frontend
permalink: archivers/handling-timeout-issues-with-angularjs-and-spring-security
---

I am working on a web application with frontend structured in a complicated way. To start with let me give you some idea of its structure. The application was originally developed using Spring at backend and GWT at frontend. The application also communicated with some third party tools and show their results on UI, results from these third party tools come in the form of HTML fragments which were then displayed using iframes.

For various obvious reasons it was decided to move the frontend of the application to AngularJS. It was changed to almost a Single Page Application (with exception of few pages). The results from third party tools was in same way displayed in iframes in Angular templates.

Now the issue was if the user timeouts navigation across different angular templates and other AJAX links did not handled this gracefully. This usually ended up in showing distorted screen with menu on left and login page embedded in iframe. The approach we decided to handle the issue was:

Add a header to all AJAX requests which will help us detect at server side that its AJAX request.

At server side check if request is AJAX and if user is not currently having access to system, add an error code 403 to response.

At angular level check if response of any request is 403 redirect the browser to login page.

The code changes thus done were:

1. To add header to all AJAX requests we did following configuration in AngularJS:

        app.config(function($httpProvider) {
          $httpProvider.defaults.headers.common = {
            ‘X-Requested-With’: ‘XMLHttpRequest
          };
        })
2. At server side we configured a filter in Spring Security to check if any Unauthorized request is AJAX and add error code to response, thus we added AjaxAuthenticationFilter with following handling for AJAX requests:

        public class AjaxAuthenticationFilter extende GenericFilterBean
          ..................
          @Override
          public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {
            try {
              chain.doFilter(request, response);
            } catch (IOException ex) {
              .............
            } catch (Exception ex) {
              .............
              if (ex instanceof AccessDeniedException) {
                String ajaxHeader = ((HttpServletRequest)request).
                  getHeader("X-Requested-With");
                if ("XMLHttpRequest".equals(ajaxHeader)) {
                  HttpServletResponse resp = (HttpServletResponse)response;
                  resp.setStatus(HttpServletResponse.SC_FORBIDDEN);
                }
              }
              ..........
            }
          }
        }
    Now add this filter to Spring Security configuration xml:

        <beans:bean id="ajaxAuthenticationFilter"
          class="com.ui.filter.AjaxAuthenticationFilter"></beans:bean>
        <http>
          <custom-filter ref="ajaxAuthenticationUIFilter"
            after="EXCEPTION_TRANSLATION_FILTER"/>
        </http>
3. Now for all HTTP responses at level of angular we will add code to to check if response code is 403 and handle it appropriately:

        app.config(function($httpProvider) {
          $provide.factory('AuthHttpInterceptor', function ($q, $window) {
            return {
              responseError: function (rejection) {
                if(rejection.status === 403)
                  $window.location.reload();
                return $q.reject(rejection);
              }
            };
          });
          $httpProvider.interceptors.push('AuthHttpInterceptor');
        })
    And this fixed issue of timeout for all our AJAX requests but we still at times had troubles when user was navigating across angular templates, the reason we discovered was that Angular templates were getting cached by browser and AJAX request was not being sent to fetch them each time user opened that page. We explored options in Angular to clear template cache but none of them guarantees that cache of HTML file in browser will be cleared. The fix we did for the issue was that at route change start we sent a dummy HTTP request to the server, at this point is the user has timed out the server will respond with HTTP code 403. And the angular configurations we have done above will take the user to login page.

        app.run(function($rootScope, $http) {
          $rootScope.$on("$routeChangeStart", function () {
            $http({method : 'GET', url : '/ui' });
          }
        });
With all these code changes and configurations finally timeout was handled gracefully for all route changes and AJAX requests in angular. The above solution was the one fitting best to our complicated frontend and most optimized in terms of network trips.
