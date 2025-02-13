## API Report File for "@backstage/plugin-permission-node"

> Do not edit this file. It is a report generated by [API Extractor](https://api-extractor.com/).

```ts
import { AllOfCriteria } from '@backstage/plugin-permission-common';
import { AnyOfCriteria } from '@backstage/plugin-permission-common';
import { AuthorizeRequestOptions } from '@backstage/plugin-permission-common';
import { BackstageIdentityResponse } from '@backstage/plugin-auth-node';
import { ConditionalPolicyDecision } from '@backstage/plugin-permission-common';
import { Config } from '@backstage/config';
import { DefinitivePolicyDecision } from '@backstage/plugin-permission-common';
import { EvaluatePermissionRequest } from '@backstage/plugin-permission-common';
import { EvaluatePermissionResponse } from '@backstage/plugin-permission-common';
import express from 'express';
import { IdentifiedPermissionMessage } from '@backstage/plugin-permission-common';
import { NotCriteria } from '@backstage/plugin-permission-common';
import { Permission } from '@backstage/plugin-permission-common';
import { PermissionAuthorizer } from '@backstage/plugin-permission-common';
import { PermissionCondition } from '@backstage/plugin-permission-common';
import { PermissionCriteria } from '@backstage/plugin-permission-common';
import { PluginEndpointDiscovery } from '@backstage/backend-common';
import { PolicyDecision } from '@backstage/plugin-permission-common';
import { TokenManager } from '@backstage/backend-common';

// @public
export type ApplyConditionsRequest = {
  items: ApplyConditionsRequestEntry[];
};

// @public
export type ApplyConditionsRequestEntry = IdentifiedPermissionMessage<{
  resourceRef: string;
  resourceType: string;
  conditions: PermissionCriteria<PermissionCondition>;
}>;

// @public
export type ApplyConditionsResponse = {
  items: ApplyConditionsResponseEntry[];
};

// @public
export type ApplyConditionsResponseEntry =
  IdentifiedPermissionMessage<DefinitivePolicyDecision>;

// @public
export type Condition<TRule> = TRule extends PermissionRule<
  any,
  any,
  infer TParams
>
  ? (...params: TParams) => PermissionCondition<TParams>
  : never;

// @public
export type Conditions<
  TRules extends Record<string, PermissionRule<any, any>>,
> = {
  [Name in keyof TRules]: Condition<TRules[Name]>;
};

// @public
export type ConditionTransformer<TQuery> = (
  conditions: PermissionCriteria<PermissionCondition>,
) => PermissionCriteria<TQuery>;

// @public
export const createConditionExports: <
  TResource,
  TRules extends Record<string, PermissionRule<TResource, any, unknown[]>>,
>(options: {
  pluginId: string;
  resourceType: string;
  rules: TRules;
}) => {
  conditions: Conditions<TRules>;
  createPolicyDecision: (
    conditions: PermissionCriteria<PermissionCondition>,
  ) => ConditionalPolicyDecision;
};

// @public
export const createConditionFactory: <TParams extends any[]>(
  rule: PermissionRule<unknown, unknown, TParams>,
) => (...params: TParams) => {
  rule: string;
  params: TParams;
};

// @public
export const createConditionTransformer: <
  TQuery,
  TRules extends PermissionRule<any, TQuery, unknown[]>[],
>(
  permissionRules: [...TRules],
) => ConditionTransformer<TQuery>;

// @public
export const createPermissionIntegrationRouter: <TResource>(options: {
  resourceType: string;
  rules: PermissionRule<TResource, any, unknown[]>[];
  getResources: (resourceRefs: string[]) => Promise<(TResource | undefined)[]>;
}) => express.Router;

// @public
export const createPermissionRule: <
  TResource,
  TQuery,
  TParams extends unknown[],
>(
  rule: PermissionRule<TResource, TQuery, TParams>,
) => PermissionRule<TResource, TQuery, TParams>;

// @alpha
export const isAndCriteria: <T>(
  criteria: PermissionCriteria<T>,
) => criteria is AllOfCriteria<T>;

// @alpha
export const isNotCriteria: <T>(
  criteria: PermissionCriteria<T>,
) => criteria is NotCriteria<T>;

// @alpha
export const isOrCriteria: <T>(
  criteria: PermissionCriteria<T>,
) => criteria is AnyOfCriteria<T>;

// @public
export const makeCreatePermissionRule: <TResource, TQuery>() => <
  TParams extends unknown[],
>(
  rule: PermissionRule<TResource, TQuery, TParams>,
) => PermissionRule<TResource, TQuery, TParams>;

// @public
export interface PermissionPolicy {
  // (undocumented)
  handle(
    request: PolicyQuery,
    user?: BackstageIdentityResponse,
  ): Promise<PolicyDecision>;
}

// @public
export type PermissionRule<
  TResource,
  TQuery,
  TParams extends unknown[] = unknown[],
> = {
  name: string;
  description: string;
  apply(resource: TResource, ...params: TParams): boolean;
  toQuery(...params: TParams): PermissionCriteria<TQuery>;
};

// @public
export type PolicyQuery = {
  permission: Permission;
};

// @public
export class ServerPermissionClient implements PermissionAuthorizer {
  // (undocumented)
  authorize(
    requests: EvaluatePermissionRequest[],
    options?: AuthorizeRequestOptions,
  ): Promise<EvaluatePermissionResponse[]>;
  // (undocumented)
  static fromConfig(
    config: Config,
    options: {
      discovery: PluginEndpointDiscovery;
      tokenManager: TokenManager;
    },
  ): ServerPermissionClient;
}
```
