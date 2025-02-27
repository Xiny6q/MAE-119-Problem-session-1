Download link :https://programming.engineering/product/mae-119-problem-session-1/

# MAE-119-Problem-session-1
MAE 119: Problem session 1
Problem 1: Theory

Find the following when it is 2:00 PM PST on Feb. 14 in San Diego, CA (32 ,-117 ):

Solar time

For Feb. 14, the day of the year is n = 45. With that, we use Eq. 1.4.2, and obtain B = (n 1) 360=365 = 43:39 . Then we obtain the equation of time E (Eq. 1.5.3)

E = 229:2(0:000075 + 0:001868 cos B 0:032077 sin B 0:014615 cos 2B 0:04089 sin 2B)

E = 14:26 min = 14 min 16 s

which is in agreement with Fig. 1.5.1 (always good to double check). Using (Eq. 1.5.2), we can compute the solar time

Solar time = Standard time + 4(Lst Lloc) + E

where Lst = 8 15 = 120 is the standard meridian for the local time zone PST (GMT-8), and the actual location longitude Lloc = 117 was given.

Solar time = 4 : 00 PM + 4 min= (120 117 ) 14 min 16 s = 2 : 00 PM 2 min 16 s = 1 : 57 : 44 PM:

Declination

The declination angle is the angular position of the sun at solar noon with respect to the plane of the equator,

it can be estimated using any of the Eqs. 1.6.1.

which is in agreement with values in Table 1.6.1.

Hour angle !

The hour angle ! is the angular displacement of the sun relative to the local meridian at 15 per hour, afternoon is positive. Since our solar time is 1:57:44 PM= 1:96 h after solar noon,

= 1:96 h 15 =h = 29:43 :

Sun angles: z, s, s

The solar zenith angle is computed using Eq. 1.6.5., with the latitude = 32

cos z = cos cos cos ! + sin sin

z = 53:62

and the solar altitude angle is its complement

s = 90

z = 36:37

Meanwhile, the solar azimuth angle is computed using Eq. 1.6.6.

= 36:38

s = sign(!) cos

1

sin z cos

cos z sinsin

depends. The convention for azimuth angles in the book is

90

Important: Isn’t this a weird value? It

facing south, 90 east, 180 or 180 north and 90 west. For class and pvlib, the convention is 180 facing south, 90 east, 0 north and 270 west. Be careful and consistent with the conventions.

1


Angle of incidence for horizontal surface? In this case, the tilt is = 0 and = z = 53:62 .

Angle of incidence for surface tilted 30 , facing south

In this case, the tilt is = 30 and = 0 (using the book convention, or 180 for pvlib). Using Eq. 1.6.2.,

cos = sin sin cos sin cos sin cos + cos cos cos cos !+ cos sin sin cos cos ! + cos sin sin sin !

= 33:1

Problem 2: pvlib

Pvlib is a software tool for solar energy applications, available for Matlab and Python (https://pvpmc.sandia. gov/applications/pv_lib-toolbox/). In the context of this class, we recommend using the Matlab pack-age. This rst exercise will focus on its installation and basic use.

Prepare your folders

Download pbliv, and extract it in the folder where you will leave your code. For example, in a ‘MAE119/code‘ folder, let’s extract the pvlib package in ‘MAE119/code/pvlib‘

Getting started

In our code folder, let’s create a script called ‘ProblemSession01.m‘. To use pvlib, the rst thing we need is to add its folder to the “PATH”, a magical place where matlab can use all known functions. We do so with this command ’addpath(genpath(’pvlib’))‘ which will add all scripts in the pvlib folder and subsequent subfolders. Now we are ready to use pvlib! (Note: There are other ways of doing this too.)

For December 25 in San Diego (32 N,117 W), obtain and plot the position of the sun during the day, including zenith, azimuth and altitude angles. Obtain and plot the air mass during the day. Obtain and plot the angle of incidence for a panel tilted 30 facing south during the day.

Add pvlib to path addpath(genpath(’pvlib’));

Setup location and time to analyze today_time=datetime(2019,12,25,0:23,0,0); % Feed in time and time zone

Time = pvl_maketimestruct(datenum(today_time),-8);

Location = pvl_makelocationstruct(32,-117); %San Diego lat and lon

%% Obtain sun position angles

[SunAz, SunEl, AppSunEl, SolarTime] = pvl_ephemeris(Time,Location); SunZen=90-SunEl;

%% Plot Solar angles

plot(today_time,SunAz,today_time,SunEl,today_time,SunZen); grid on legend(’Azimuth angle’,’Elevation angle’,’Zenith angle’) xlabel(’Hour of day’); ylabel(’Angle (deg)’)

% Why is elevation negative?

Another way of plotting (as we’ve seen in class) plot(SunAz,SunEl); grid on

xlabel(’Azimuth angle (deg)’); ylabel(’Elevation angle(deg)’)

Obtain air mass

AMa = pvl_relativeairmass(SunZen);

%there is also an absolute air mass function, requires pressure


%% Plot air mass

plot(today_time,AMa); xlabel(’Time’); ylabel(’Air mass’); % there are nan values… why?

Setup solar panel orientation and get angle of incidence SurfTilt = 30; % Array tilt angle (deg)

SurfAz = 180; %Array azimuth (180 deg indicates array faces South) AOI=pvl_getaoi(SurfTilt,SurfAz,SunZen,SunAz);

Plot AOI

plot(today_time,AOI); xlabel(’Time’); ylabel(’Angle of incidence (deg)’)
