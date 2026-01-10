
```js
{
  Model: {
    fields: {
      id: type.string().uuid(4),
      name: type.string().alphanum().min(1).max(maxLabelLength).required(),
      accountId: type.string().uuid(4).required(),
      position: type.number().integer().required(),
      // Access control start
      // General access
      accessTokenAuthField: type.string().min(1).max(maxLabelLength),
      accessModelAuthField: type.string().min(1).max(maxLabelLength),
      accessEnableMultipleOwners: type.boolean(),
      accessAllowByDefaultOwners: type.boolean(),
      accessCreate: type.string().enum(accessModesEnum).allowNull(),
      accessRead: type.string().enum(accessModesEnum).allowNull(),
      accessUpdate: type.string().enum(accessModesEnum).allowNull(),
      accessDelete: type.string().enum(accessModesEnum).allowNull(),
      // Action-specific access
      accessEnableMultipleCreators: type.boolean(),
      accessAllowByDefaultCreators: type.boolean(),
      accessEnableMultipleReaders: type.boolean(),
      accessAllowByDefaultReaders: type.boolean(),
      accessEnableMultipleUpdaters: type.boolean(),
      accessAllowByDefaultUpdaters: type.boolean(),
      accessEnableMultipleDeleters: type.boolean(),
      accessAllowByDefaultDeleters: type.boolean(),
      accessCreateTokenAuthField: type.string().min(1).max(maxLabelLength),
      accessReadTokenAuthField: type.string().min(1).max(maxLabelLength),
      accessUpdateTokenAuthField: type.string().min(1).max(maxLabelLength),
      accessDeleteTokenAuthField: type.string().min(1).max(maxLabelLength),
      accessCreateModelAuthField: type.string().min(1).max(maxLabelLength),
      accessReadModelAuthField: type.string().min(1).max(maxLabelLength),
      accessUpdateModelAuthField: type.string().min(1).max(maxLabelLength),
      accessDeleteModelAuthField: type.string().min(1).max(maxLabelLength),
      // Access control end
      createdAt: type.number().required(),
      updatedAt: type.number().required()
    },
    indexes: [
      'accountId',
      {
        name: 'accountIdName',
        type: 'compound',
        fn: function (r) {
          return [ r.row('accountId'), r.row('name') ];
        }
      }
    ],
    views: {
      accountAlphabeticalView: {
        paramFields: [ 'accountId' ],
        primaryFields: [ 'accountId' ],
        affectingFields: [ 'name' ],
        transform: function (fullTableQuery, r, params) {
          return fullTableQuery
            .getAll(params.accountId, { index: 'accountId' })
            .orderBy(r.asc('name'));
        }
      },
      accountCustomPositionView: {
        paramFields: [ 'accountId' ],
        primaryFields: [ 'accountId' ],
        affectingFields: [ 'position' ],
        transform: function (fullTableQuery, r, params) {
          return fullTableQuery
            .getAll(params.accountId, { index: 'accountId' })
            .orderBy('position');
        }
      },
      accountModelWithNameView: {
        paramFields: [ 'accountId', 'name' ],
        primaryFields: [ 'accountId', 'name' ],
        transform: function (fullTableQuery, r, params) {
          return fullTableQuery
            .getAll([params.accountId, params.name], { index: 'accountIdName' })
            .orderBy(r.asc('name'));
        }
      }
    }
  },
  ModelField: {
    fields: {
      id: type.string().uuid(4),
      name: type.string().alphanum().min(1).max(maxLabelLength).required(),
      type: type.string().enum([
        'string',
        'number',
        'boolean'
      ]).required(),
      // Constraints start
      required: type.boolean(),
      allowNull: type.boolean(),
      min: type.number(),
      max: type.number(),
      length: type.number(),
      alphanum: type.boolean(),
      regex: type.string().max(maxRegExpStringLength),
      regexFlags: type.string().max(maxRegExpFlagStringLength),
      email: type.boolean(),
      lowercase: type.boolean(),
      uppercase: type.boolean(),
      enum: type.string().max(maxEnumStringLength),
      uuid: type.boolean(),
      integer: type.boolean(),
      multi: type.boolean(),
      blob: type.boolean(),
      visibilityCondition: type.string().min(1).max(maxExpressionLength),
      defaultValue: type.string().max(maxDefaultValueLength),
      // Constraints end
      accountId: type.string().uuid(4).required(),
      modelId: type.string().uuid(4).required(),
      position: type.number().integer().required(),
      // Access control start
      accessCreate: type.string().enum(accessModesEnum).allowNull(),
      accessRead: type.string().enum(accessModesEnum).allowNull(),
      accessUpdate: type.string().enum(accessModesEnum).allowNull(),
      accessDelete: type.string().enum(accessModesEnum).allowNull(),
      // Access control end
      maxCardinality: type.number().integer(),
      referentialIntegrityModel: type.string().alphanum().min(1).max(maxLabelLength),
      createdAt: type.number().required(),
      updatedAt: type.number().required()
    },
    indexes: [
      'accountId',
      'modelId',
      {
        name: 'accountIdModelId',
        type: 'compound',
        fn: function (r) {
          return [ r.row('accountId'), r.row('modelId') ];
        }
      },
      {
        name: 'accountIdModelIdName',
        type: 'compound',
        fn: function (r) {
          return [ r.row('accountId'), r.row('modelId'), r.row('name') ];
        }
      }
    ],
    views: {
      accountModelAlphabeticalView: {
        paramFields: [ 'accountId', 'modelId' ],
        primaryFields: [ 'accountId', 'modelId' ],
        affectingFields: [ 'name' ],
        transform: function (fullTableQuery, r, params) {
          return fullTableQuery
            .getAll([ params.accountId, params.modelId ], { index: 'accountIdModelId' })
            .orderBy(r.asc('name'));
        }
      },
      accountModelCustomPositionView: {
        paramFields: [ 'accountId', 'modelId' ],
        primaryFields: [ 'accountId', 'modelId' ],
        affectingFields: [ 'position' ],
        transform: function (fullTableQuery, r, params) {
          return fullTableQuery
            .getAll([ params.accountId, params.modelId ], { index: 'accountIdModelId' })
            .orderBy('position');
        }
      },
      accountModelNameView: {
        paramFields: [ 'accountId', 'modelId', 'name' ],
        primaryFields: [ 'accountId', 'modelId', 'name' ],
        transform: function (fullTableQuery, r, params) {
          return fullTableQuery
            .getAll([ params.accountId, params.modelId, params.name ], { index: 'accountIdModelIdName' });
        }
      }
    }
  },
  ModelIndex: {
    fields: {
      id: type.string().uuid(4),
      name: type.string().min(1).max(maxIndexNameLength).required(),
      fields: type.string().min(1).max(maxIndexDefinitionLength).required(),
      format: type.string().enum(indexFormats).allowNull(),
      accountId: type.string().uuid(4).required(),
      modelId: type.string().uuid(4).required(),
      builtAt: type.number(),
      maxCardinality: type.number().integer(),
      createdAt: type.number().required(),
      updatedAt: type.number().required()
    },
    indexes: [
      'accountId',
      'modelId',
      {
        name: 'accountIdModelId',
        type: 'compound',
        fn: function (r) {
          return [ r.row('accountId'), r.row('modelId') ];
        }
      },
      {
        name: 'accountIdModelIdName',
        type: 'compound',
        fn: function (r) {
          return [ r.row('accountId'), r.row('modelId'), r.row('name') ];
        }
      }
    ],
    views: {
      accountModelAlphabeticalView: {
        paramFields: [ 'accountId', 'modelId' ],
        primaryFields: [ 'accountId', 'modelId' ],
        affectingFields: [ 'name', 'builtAt' ],
        transform: function (fullTableQuery, r, params) {
          return fullTableQuery
            .getAll([ params.accountId, params.modelId ], { index: 'accountIdModelId' })
            .orderBy(r.asc('name'));
        }
      },
      accountModelNameView: {
        paramFields: [ 'accountId', 'modelId', 'name' ],
        primaryFields: [ 'accountId', 'modelId', 'name' ],
        transform: function (fullTableQuery, r, params) {
          return fullTableQuery
            .getAll([ params.accountId, params.modelId, params.name ], { index: 'accountIdModelIdName' });
        }
      }
    }
  },
  ModelView: {
    fields: {
      id: type.string().uuid(4),
      name: type.string().min(1).max(maxIndexNameLength).required(),
      paramFields: type.string().max(maxViewParamLength),
      primaryFields: type.string().max(maxViewParamLength),
      affectingFields: type.string().max(maxViewParamLength),
      transformIndex: type.string().max(maxViewTransformIndexLength),
      transformIndexOperation: type.string().max(maxViewTransformOperationTypeLength),
      transformIndexOperationMulti: type.boolean(),
      transformIndexOperationInputA: type.string().max(maxViewTransformInputLength),
      transformIndexOperationInputB: type.string().max(maxViewTransformInputLength),
      transformFilterType: type.string().enum(transformFilterTypes),
      transformFilterField: type.string().max(maxViewTransformFieldLength),
      transformFilterOperation: type.string().max(maxViewTransformOperationTypeLength),
      transformFilterOperationInput: type.string().max(maxViewTransformInputLength),
      transformFilterQuery: type.string().max(maxSearchFilterLength),
      transformOrderByField: type.string().max(maxViewTransformOrderByFieldLength),
      transformOrderByDesc: type.boolean(),
      transformDistinct: type.boolean(),
      transformIndexOperationLeftBoundOpen: type.boolean(),
      transformIndexOperationRightBoundClosed: type.boolean(),
      disableRealtime: type.boolean(),
      maxOffset: type.number(),
      hideFieldsByDefault: type.boolean(),
      showFields: type.string().min(1).max(maxFieldListLength),
      hideFields: type.string().min(1).max(maxFieldListLength),
      hiddenFieldValues: type.string().min(1).max(maxFieldListLength),
      accountId: type.string().uuid(4).required(),
      modelId: type.string().uuid(4).required(),
      createdAt: type.number().required(),
      updatedAt: type.number().required()
    },
    indexes: [
      'accountId',
      'modelId',
      {
        name: 'accountIdModelId',
        type: 'compound',
        fn: function (r) {
          return [ r.row('accountId'), r.row('modelId') ];
        }
      },
      {
        name: 'accountIdModelIdName',
        type: 'compound',
        fn: function (r) {
          return [ r.row('accountId'), r.row('modelId'), r.row('name') ];
        }
      }
    ],
    views: {
      accountModelAlphabeticalView: {
        paramFields: [ 'accountId', 'modelId' ],
        primaryFields: [ 'accountId', 'modelId' ],
        affectingFields: [ 'name' ],
        transform: function (fullTableQuery, r, params) {
          return fullTableQuery
            .getAll([ params.accountId, params.modelId ], { index: 'accountIdModelId' })
            .orderBy(r.asc('name'));
        }
      },
      accountModelNameView: {
        paramFields: [ 'accountId', 'modelId', 'name' ],
        primaryFields: [ 'accountId', 'modelId', 'name' ],
        transform: function (fullTableQuery, r, params) {
          return fullTableQuery
            .getAll([ params.accountId, params.modelId, params.name ], { index: 'accountIdModelIdName' })
            .orderBy(r.asc('name'));
        }
      }
    }
  }
}
```